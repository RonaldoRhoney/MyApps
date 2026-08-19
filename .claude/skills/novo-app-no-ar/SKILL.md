---
description: Checklist obrigatório sempre que um app RhoneyInc (novo ou já existente) entra no ar publicamente — registro no hub, subdomínio padrão, rodapé, ícone. Use assim que um deploy de produção for confirmado (ex: "site tal está no ar", "publiquei o Vercel", "deploy feito").
---

# App novo no ar — checklist do hub RhoneyInc

Regra do Ronaldo (2026-07-25): assim que qualquer app RhoneyInc for publicado em produção — mesmo que ainda incompleto —, aplique este checklist **diretamente, sem esperar pedido explícito**. O objetivo é o site `rhoneyinc.com` nunca ficar desatualizado em relação ao que já está no ar de verdade.

## Contexto: como o hub funciona hoje

A lista pública de produtos do `rhoneyinc.com` **não é o rodapé estático** — é a tabela `softwares` no Supabase do próprio RhoneyInc hub (projeto `crkryabvsmlraizaurnk`, arquivo `RhoneyInc/schema.sql`). O painel ADM embutido no site (`RhoneyInc/index.html`, seção `#admin`, lógica em `RhoneyInc/admin.js`) lê/escreve essa tabela via formulário "Adicionar/editar software". O rodapé (`footer` em `index.html`) é HTML fixo, **duplicado manualmente** — não é gerado a partir da tabela.

Colunas relevantes de `softwares`: `nome`, `descricao`, `status` (`disponivel` | `em_desenvolvimento`), `plataforma`, `link_url`, `logo_url`, `cor_acento`, `ativo`, `ordem`.

## Checklist ao publicar um app (ou corrigir um já publicado)

1. **Subdomínio padrão**: confirme que o app está em `{produto}.rhoneyinc.com`, não num domínio genérico tipo `*.vercel.app`. `rhoneyinc.com` é registrado no próprio Vercel (`vercel domains ls`), então basta `vercel domains add {produto}.rhoneyinc.com {nome-do-projeto-vercel}` — não precisa mexer em DNS externo. **AmaVida migrou pra `amavida.rhoneyinc.com` em 2026-07-26** (pedido explícito do Ronaldo, revertendo a exceção anterior). **MeuPet continua de propósito em `meupet-zeta.vercel.app`** — não mude sem perguntar de novo.
2. **Ícone**: se não existir `{produto}-icon.svg` em `RhoneyInc/assets/`, crie um seguindo o padrão dos existentes (viewBox 192x192, `rx="42"`, fundo escuro derivado da cor de acento do produto, glow radial com a cor de acento, símbolo simples no centro). Veja `menuflex-icon.svg`, `vagalume-icon.svg` como referência.
3. **Registro em `softwares`**: insira (ou atualize) a linha do produto — `status='disponivel'` só se o app estiver de fato pronto pro público (não confunda "tem deploy" com "está pronto" — confirme com o Ronaldo se não estiver óbvio, como aconteceu com AmaVida). Conecta via `psql` direto (peça a senha do banco, mesmo fluxo usado em VagaLume/API-Futebol) ou via sessão autenticada de admin no navegador.
4. **Rodapé do hub**: adicione o link em `RhoneyInc/index.html`, dentro de `.footer-col` "Produtos", mesma ordem/estilo dos outros — só produtos `disponivel` entram aqui; os `em_desenvolvimento` já aparecem via `#softwares`.
5. **Rodapé do próprio produto**: confirme que segue a skill [[footer-padrao]] — 4 colunas fixas, nunca uma estrutura inventada. Atualize a tabela de status daquela skill.
6. **Painel ADM com métricas do produto**: hoje só existe bloco de métricas pra MeuPet (hardcoded em `index.html`/`admin.js`) — os demais produtos ainda não têm métricas agregadas no hub. Isso é uma iniciativa maior e separada (integrar Supabase de cada produto ao painel do hub), não faça isso "de passagem" junto com o registro básico — trate como projeto à parte quando for pedido.

## O que NUNCA fazer sem confirmar

- Mudar `status` de `em_desenvolvimento` pra `disponivel` só porque um deploy existe — "no ar" e "pronto pro público" são coisas diferentes (caso AmaVida — o domínio já é `rhoneyinc.com`, mas o status continua `em_desenvolvimento` até o Ronaldo confirmar o contrário).
- Adicionar subdomínio `rhoneyinc.com` a um app que o dono decidiu deixar fora desse padrão (caso MeuPet, confirmado 2026-07-25).
- Inventar uma URL de produção que você não verificou rodando (`curl -I` ou `vercel project ls`) — MontaMovel, por exemplo, ainda não tem deploy real apesar da pasta existir.
