---
description: Padrão oficial de engenharia, arquitetura, documentação e execução de TODO produto RhoneyInc (novo ou existente) — discovery antes de código, .claude como fonte oficial de instrução, APIs públicas/gratuitas antes de pagas, fundação antes de feature, versão por etapas, nunca desviar da arquitetura sem registrar. Use ao iniciar qualquer projeto novo dentro de MyApps, ao entrar num projeto RhoneyInc existente pela primeira vez na sessão, e antes de qualquer tarefa significativa (não trivial) em produto RhoneyInc.
---

# Padrão RhoneyInc de projeto

Regra transversal do Ronaldo (2026-08-15): nenhum produto RhoneyInc — atual ou futuro — nasce ou evolui de forma improvisada. Vale independente de stack (Python/FastAPI, JS/TS, React, Android/Kotlin, Postgres/Supabase/SQLite, Docker, ou o que vier depois). **A tecnologia muda; estes princípios não.**

> **Princípio fundamental**: todo produto RhoneyInc deve nascer de uma compreensão arquitetural clara antes da implementação. Não comece criando tela, componente, tabela ou integração só porque o pedido descreveu uma ideia.

Esta skill é o guarda-chuva. Ela não substitui [[novo-projeto]] (plano antes de código), [[verificar-premissas]] (checar afirmações contra o real antes de executar brief técnico) nem [[vibe-coding-5-falhas]] (checklist de segurança pré-deploy) — reforça o mesmo espírito e remete a elas em vez de duplicar.

## 1. Antes de qualquer coisa: ler o `.claude` do projeto

A pasta `.claude` (e o `CLAUDE.md` do projeto, quando existir) é **fonte oficial de instrução** daquele produto — não uma sugestão.

1. Localize `.claude/` e `CLAUDE.md` na raiz do projeto (cada produto RhoneyInc dentro de `MyApps/` pode ter o seu, além deste `.claude` de nível `MyApps/`).
2. Leia as instruções, liste as Skills disponíveis ali (podem existir Skills específicas do produto, ex: `menuflex-security`, `vendeflex-docs`).
3. Identifique documentos de fundação já existentes (ex: `docs/foundation/*.md`, `DECISIONS.md`, `ARCHITECTURE.md`) e decisões arquiteturais já tomadas.
4. Respeite o que já está definido. Nunca ignore uma regra existente porque outra abordagem parece mais conveniente no momento.
5. Se houver conflito entre instruções (deste guarda-chuva, do `CLAUDE.md` do produto, ou de um pedido do Ronaldo): identifique o conflito, explique-o, aponte a hierarquia de prioridade que você está aplicando, e **nunca apague ou altere regra existente silenciosamente**. Peça orientação quando a prioridade não for óbvia.

## 2. APIs e serviços externos: público/gratuito antes de pago

Quando o produto precisar de dado ou serviço externo, avalie nesta ordem antes de integrar algo pago: API pública → API gratuita → fonte pública oficial → dataset público → serviço open source → solução local/self-hosted → cache → dado mockado (só pra desenvolvimento, nunca apresentado como real em produção).

**Nunca assuma que uma API é gratuita só porque tem documentação pública.** Antes de integrar, verifique: autenticação exigida, rate limits, custo real por uso/volume, política de uso (permite produção? permite uso comercial?), necessidade de cartão/assinatura, estabilidade, licença.

Se a única opção viável exigir pagamento real por consumo, **não integre silenciosamente**. Apresente: qual serviço foi identificado, qual seria o custo, por que é necessário, quais alternativas gratuitas foram avaliadas e por que não serviram — e peça decisão.

Achado real já registrado no ecossistema: `ip-api.com` (geolocalização, usado no KnowRa) tem tier gratuito que **proíbe uso comercial** nos termos — aceitável enquanto o produto não monetiza, mas precisa revisão antes de virar produto pago. Esse é exatamente o tipo de letra miúda que essa checagem existe pra pegar antes, não depois.

## 3. Novo projeto: protocolo de discovery → fundação → implementação

Para projeto novo (ou uma fase/etapa nova claramente grande dentro de um produto existente), siga nesta ordem — cada fase produz algo concreto antes da próxima começar:

1. **Discovery** — problema, objetivo, público, proposta de valor, funcionalidades, regras de negócio, integrações, dados, segurança, requisitos não-funcionais, restrições, riscos. Sem código ainda.
2. **Contexto** — documentar visão, problema/solução, escopo e fora-de-escopo, usuários, fluxos principais, premissas, restrições.
3. **Arquitetura** — stack, módulos, frontend/backend/banco, APIs, autenticação/autorização, armazenamento, integrações, infraestrutura, segurança, observabilidade. Se a stack proposta divergir do padrão de fato do ecossistema (hoje: Supabase como banco+auth, deploy Vercel, React/Vite/TS quando o produto usa frontend próprio), justifique a divergência em vez de trocar silenciosamente — mesmo espírito da decisão registrada no KnowRa de usar Supabase Auth em vez de Auth Service customizado.
4. **Roadmap por versão/etapa** — divida o desenvolvimento (ex: v0.1 Foundation → v0.2 Core → v0.3 Intelligence → v0.4 Integrations → v0.5 Optimization, ou Etapa 1 Fundação → ... → Etapa 6 Produção). Escolha o formato mais natural pro projeto, mas sempre etapado — nunca "implementa tudo de uma vez".
5. **Fundação** — antes de qualquer funcionalidade: estrutura de diretórios, configuração, dependências, padrões de código, tratamento de erro, segurança base, contratos/modelos/interfaces, documentação inicial.
6. **Implementação** — só o escopo aprovado da versão/etapa atual. Não antecipe funcionalidade de versão futura sem necessidade real.
7. **Testes** — apropriados ao que foi construído (unitário, integração, API, banco, fluxo crítico, segurança, regressão).
8. **Auditoria** — antes de declarar a versão concluída, confira: requisitos atendidos, arquitetura respeitada, segurança (aplique [[vibe-coding-5-falhas]] antes de deploy), erros tratados, integrações validadas, documentação atualizada, UX, código duplicado, dependências, custo de API externa, performance quando relevante.
9. **Versionamento/registro** — o que foi implementado, o que mudou, problemas encontrados, decisões técnicas, limitações, próximo passo. Em produtos com `DECISIONS.md` (padrão já usado em KnowRa, VoaRadar), registre lá.

`REQUISITOS + IMPLEMENTAÇÃO + TESTES + AUDITORIA + DOCUMENTAÇÃO` coerentes entre si — só então uma versão está concluída. "Funciona" sozinho não é sinônimo de "versão concluída".

## 4. Documentação: estrutura de referência, não obrigação de preencher tudo

Quando o projeto justificar documentação formal, apoie-se nesta estrutura (adapte, não force o que não fizer sentido pro tamanho do projeto):

```text
docs/
├── PROJECT_CONTEXT.md   (ou PRODUCT.md/VISION.md, ver KnowRa)
├── ARCHITECTURE.md
├── ROADMAP.md
├── DATA_MODEL.md
├── SECURITY.md
├── DECISIONS.md
└── (CHANGELOG.md / versions/ quando o projeto tiver ciclo de release formal)
```

Não crie documento só pra preencher a estrutura — ela deve refletir o estado real do projeto. Um projeto pequeno pode viver só com um `CLAUDE.md` enxuto; um projeto do porte do KnowRa justifica o conjunto completo em `docs/foundation/`.

## 5. Não desviar da arquitetura sem registrar

Se durante a implementação surgir uma necessidade fora do plano/arquitetura aprovada:

1. Identifique o problema concretamente.
2. Avalie o impacto (o que quebra, o que fica mais complexo, o que fica mais simples).
3. Proponha a solução — não decida e aplique em silêncio.
4. Verifique se pertence ao escopo da versão/etapa atual ou se é matéria pra depois.
5. Registre a decisão (em `DECISIONS.md` do produto, ou na resposta ao Ronaldo se o produto não tiver esse arquivo ainda).
6. Só então implemente.

Ajuste pequeno e claramente necessário pra fechar o escopo atual pode ser feito sem parar tudo — mas **precisa ser documentado**, nunca silencioso.

## 6. O que evitar

- Começar direto pelo código sem entender o problema.
- Criar arquitetura improvisada ou instalar dependência sem necessidade comprovada.
- Integrar API paga sem antes avaliar alternativa pública/gratuita (§2).
- Ignorar `.claude`/`CLAUDE.md`/documentação de fundação já existente no projeto.
- Duplicar funcionalidade que já existe (confira antes — mesmo raciocínio de [[verificar-premissas]]).
- Alterar integração existente ou regra de negócio sem avaliar impacto e registrar.
- Misturar escopo de versões/etapas diferentes numa única implementação.
- Declarar etapa/versão concluída sem auditoria (§3.8).
- Apresentar dado fictício/mockado como se fosse real em produção.

## 7. Ao receber um projeto novo: como responder antes de implementar

1. **Entendimento do projeto**
2. **Objetivo**
3. **Escopo inicial**
4. **Stack sugerida** (e onde diverge do padrão de fato do ecossistema, se divergir)
5. **Arquitetura proposta**
6. **APIs/serviços externos** (com a avaliação do §2)
7. **Estratégia de custo mínimo**
8. **Estrutura de fundação**
9. **Roadmap de versões/etapas**
10. **Próximo passo**

Depois de aprovado (ou quando a tarefa já tiver autorização explícita suficiente, ex: "pode seguir" numa etapa já contextualizada), iniciar a implementação — sem reabrir esse protocolo inteiro a cada etapa incremental já combinada.
