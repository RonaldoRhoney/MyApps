---
description: Checklist de segurança contra as 5 falhas mais comuns em apps construídos com IA/vibe coding (RLS desativado, permissão no front-end, IDOR, chaves expostas, XSS). Use antes de deploy ou quando o usuário pedir auditoria de segurança em qualquer produto RhoneyInc.
---

# As 5 falhas de vibe coding

Baseado no vídeo "USOU VIBECODING? TÁ CORRENDO RISCO" (Mano Deyvin) — resumo trazido pelo Ronaldo em 2026-08-13, aplicado pela primeira vez no Voa Radar. Regra: rodar esse checklist em **qualquer produto RhoneyInc** antes de deploy, e sempre que for pedida uma "auditoria de segurança" — não é exclusivo do Voa Radar.

Todo achado precisa ser confirmado com verificação real (query no banco, grep no código, teste ao vivo) — nunca marcar um item como resolvido só por não lembrar de ter feito diferente.

## 1. RLS (Row Level Security) desativado

**O problema**: banco fica exposto sem regra que limite o acesso — em projetos Supabase, toda tabela do schema `public` é auto-exposta pela API REST (PostgREST) pros papéis `anon`/`authenticated`, com grants completos por padrão quando a tabela é criada fora do dashboard (ex: via migration direta).

**Como verificar** (rodar contra o banco real, com a conexão que o backend usa):

```sql
-- RLS habilitado?
SELECT relname, relrowsecurity FROM pg_class
WHERE relnamespace = 'public'::regnamespace;

-- Grants perigosos pra anon/authenticated (deve vir vazio)
SELECT table_name, grantee, privilege_type
FROM information_schema.role_table_grants
WHERE table_schema = 'public' AND grantee IN ('anon','authenticated');

-- O papel do backend bypassa RLS? (confirma que a correção não quebra a app)
SELECT rolname, rolbypassrls FROM pg_roles WHERE rolname = current_user;
```

**Não esquecer**: a tabela `alembic_version` (ou equivalente de outra ferramenta de migration) também é criada no schema `public` e também precisa ser travada — é fácil esquecer dela por não ser uma tabela "de negócio".

**Correção**: `ALTER TABLE <tabela> ENABLE ROW LEVEL SECURITY;` + `REVOKE ALL ON TABLE <tabela> FROM anon, authenticated;` em toda tabela nova, como parte da própria migration que cria a tabela — não como um passo separado que pode ser esquecido depois.

## 2. Lógica de permissão no front-end

**O problema**: decidir quem é admin (ou qualquer controle de acesso) no navegador — `localStorage`, uma flag em contexto React, uma checagem de `role` no client — é sempre manipulável pelo usuário (DevTools, localStorage editável).

**Como verificar**:

```bash
grep -rniE "localStorage|sessionStorage|isAdmin|is_admin|role\s*===|permission" frontend/src --include="*.ts" --include="*.tsx"
```

Qualquer resultado que não seja puramente de UI (ex: "mostrar botão X se..." sem consequência de segurança real) merece investigação. A regra: toda decisão que importa de verdade (o que o usuário pode ver/fazer/alterar) tem que ser **imposta no backend**, o front-end no máximo reage ao que o backend já decidiu.

## 3. Ataque IDOR (Insecure Direct Object Reference)

**O problema**: endpoint recebe um ID (`/recurso/{id}`) e não confere se aquele recurso pertence a quem está pedindo — outro usuário troca o ID na URL e acessa dado alheio.

**Como verificar**: listar todo endpoint que recebe um ID de recurso e perguntar: existe conceito de "dono" desse recurso? Se sim, o código confere `resource.owner_id == current_user.id` (ou equivalente) antes de devolver/alterar?

**Importante**: em produtos sem login ainda (ex: Voa Radar até v0.3), IDOR clássico não se aplica formalmente — não existe usuário dono de nada ainda. Mas isso muda no dia em que login/dado pessoal entrar (buscas salvas, alertas, perfil) — nesse momento, todo endpoint que já existia e passa a lidar com dado de usuário precisa ser revisto por esse ângulo, não só os endpoints novos.

## 4. Chaves de API expostas

**O problema**: chave hardcoded no código que vai pro repositório, ou exposta no bundle do client (qualquer variável sem prefixo específico do bundler acaba embutida no JS que o navegador baixa).

**Como verificar**:

```bash
# Segredo hardcoded em qualquer lugar do repo
grep -rniE "(api[_-]?key|secret[_-]?key|password|token)\s*[:=]\s*[\"'][a-zA-Z0-9]" --include="*.py" --include="*.ts" --include="*.tsx" --include="*.js" .

# .env já foi commitado alguma vez?
git log --all --full-history -- "*.env"

# O build de produção do frontend não deve conter nada sensível
grep -rE "sk-|SERVICE_ROLE|SECRET" frontend/dist/
```

Confirmar que `.env` está no `.gitignore` desde o primeiro commit do projeto (não só a partir de agora), e que `.env.example` só tem placeholders.

## 5. Falta de tratamento de input (XSS)

**O problema**: confiar cegamente em dado enviado pelo usuário e renderizar sem escapar, permitindo execução de script.

**Como verificar**:

```bash
grep -rn "dangerouslySetInnerHTML\|innerHTML\|eval(" frontend/src --include="*.ts" --include="*.tsx"
```

Framework React/Vue por padrão já escapa interpolação de texto — o risco real está em `dangerouslySetInnerHTML` (React), `v-html` (Vue), ou manipulação direta do DOM. Teste positivo de verdade (não só ausência de padrão perigoso): mandar um payload real (`<script>alert(1)</script>`) num campo de texto livre e confirmar que aparece como texto na tela, não executa.

## Ferramentas de apoio (mencionadas no vídeo)

- **Bandit** (`pip install bandit && bandit -r app/`) — análise estática de segurança em Python. Rodar no backend a cada auditoria.
- **oxlint/eslint** — já rodado como lint padrão do frontend; não é focado em segurança, mas pega alguns padrões perigosos.
- **Gitleaks** — scanner de segredo no histórico do Git (binário, não instalado por padrão nesse ambiente — avaliar instalação se o achado do grep manual for insuficiente).
- **OWASP ZAP** — scanner de aplicação web em execução (mais pesado, avaliar caso a caso, não é rotina de toda auditoria).

## Status por produto (última checagem)

| Produto | RLS | Permissão no front | IDOR | Chaves expostas | XSS | Data |
|---|---|---|---|---|---|---|
| Voa Radar | ✅ Corrigido — auditoria formal FASE 9 (10 tabelas, RLS ativo em todas, `anon` sem grant nenhum, `authenticated` só com o escopo pretendido por tabela, 5 policies reais por `auth.uid()`, backend com `rolbypassrls`) | ✅ Testado de verdade — grep confirma zero decisão de permissão no front (único uso de `localStorage` é guardar o token de sessão, não decidir autorização); toda autorização imposta no backend | ✅ Testado de verdade — 10/10 testes de IDOR automatizados: usuário B sempre recebe 404 (nunca 403) ao tentar ler/editar/apagar Radar ou notificação do usuário A | ✅ Limpo (grep no repo + `git log` do `.env` + grep no `dist/` de produção + Bandit) | ✅ Limpo (grep confirma zero `dangerouslySetInnerHTML`/`innerHTML`/`eval` em todo o código da v0.4) | 2026-08-14 (v0.4 FASE 9 — auditoria formal completa, ver docs/v0.4/SECURITY.md §6) |

Atualize esta tabela sempre que rodar o checklist em outro produto.
