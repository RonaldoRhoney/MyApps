---
name: google-play-policies
description: Regras e políticas do Google Play Console que TODO app da RhoneyInc precisa cumprir antes de ser publicado ou atualizado. Use esta skill sempre que o usuário estiver planejando, desenvolvendo, revisando ou publicando qualquer app Android/TWA (MeuPet, FinWise, FitNow, MontaMovel, AmaVida, MenuFlex, Até Passar, ou qualquer novo produto), sempre que mencionar Play Console, Play Store, publicação, submissão, revisão de app, política de privacidade, permissões Android, target SDK, classificação de conteúdo, dados de usuário, verificação de desenvolvedor, ou integração de IA de terceiros (Claude/Anthropic) dentro de um app. Consulte esta skill ANTES de escrever qualquer prompt de desenvolvimento, plano de MVP ou código que toque em: coleta de dados pessoais, permissões sensíveis (SMS, localização, câmera, contatos), chat com IA embutido, cadastro/autenticação de usuários, ou apps voltados a crianças/família. Também use para auditar apps já publicados quanto à conformidade.
---

# Políticas do Google Play — RhoneyInc

Skill de referência obrigatória para todos os produtos da RhoneyInc (MeuPet, FinWise, FitNow, MontaMovel, AmaVida, MenuFlex, Até Passar, Football Results API e futuros apps). Consolida as políticas do Google Play Console mais relevantes para o stack da RhoneyInc (React + Supabase + Python/FastAPI + Lovable + Claude AI + n8n, deploy via Vercel/TWA-Bubblewrap), com foco no **Aviso de Políticas de 15/07/2026** e nas políticas permanentes mais aplicáveis ao portfólio.

**Regra de ouro da RhoneyInc**: nenhum app é considerado "pronto para publicar" sem passar pelo checklist desta skill. Trate isso como pré-requisito, no mesmo nível dos padrões de código (responsividade, footer "Copyright @RhoneyInc", plano antes de codar).

---

## 1. Mudanças da atualização de 15/07/2026 (prazo: pelo menos 30 dias a partir dessa data)

| Mudança | Aplica a quem? | Ação necessária |
|---|---|---|
| **Dados de Usuário + integrações de IA de terceiros**: fica explícito que apps que integram IA de terceiros (ex.: Claude/Anthropic) são responsáveis por uso limitado, divulgação e consentimento dos dados enviados a essa IA. | **FinWise** (agentes de IA por módulo), **MontaMovel** (chat flutuante Claude por módulo), qualquer app futuro com chatbot/IA embutida | Ver seção 3 abaixo — é o item de MAIOR prioridade para a RhoneyInc. |
| **Verificação de desenvolvedor obrigatória** (Android Developer Verification + registro no Play Console) | Todos os apps, mesmo os distribuídos fora da Play Store | Verificar em Play Console > Developer Verification se cada app está registrado. MeuPet já está em processo — replicar para os demais antes do lançamento. |
| **Target API level obrigatório até 31/08/2026** | Todos os apps Android/TWA publicados (MeuPet, e futuros TWA de FinWise etc.) | Confirmar `targetSdkVersion` no `AndroidManifest.xml` / config do Bubblewrap contra o nível mínimo exigido no ano corrente. |
| **Apps sem classificação de conteúdo não são mais permitidos** | Todos os apps | Preencher o questionário de Classificação de Conteúdo no Play Console antes de qualquer submissão/atualização. |
| Chat anônimo/aleatório: novas restrições de segurança infantil | Não se aplica a nenhum produto atual da RhoneyInc (nenhum é chat anônimo) | Nenhuma ação — apenas monitorar se algum produto futuro (ex.: features sociais do MeuPet) evoluir para chat 1:1 anônimo. |
| `READ_CALL_LOG` não é mais aceito para verificação de conta por ligação | Só relevante se algum app usar essa permissão para 2FA por chamada | Nenhum produto atual usa isso. Se implementar verificação por telefone no futuro, usar Digital Credentials API ou SMS Retriever API — nunca `READ_CALL_LOG`. |
| Empréstimos Pessoais / EWA (antecipação salarial) — esclarecimento, sem regra nova | Só se FinWise adicionar produto de antecipação de salário | Hoje não se aplica (FinWise é gestão financeira pessoal, não EWA). Reavaliar se essa feature for adicionada. |
| Novo esclarecimento sobre local exato/aproximado na seção "Segurança dos dados" | MeuPet (geolocalização de petshops via GPS + IP fallback) | Conferir se a declaração de "Segurança dos dados" no Play Console distingue local aproximado vs. exato corretamente. |

---

## 2. Checklist obrigatório antes de QUALQUER submissão/atualização no Play Console

Rode este checklist para cada app antes de enviar para revisão:

1. **Política de Privacidade**
   - URL ativa, específica ao app, publicada tanto na ficha da loja quanto dentro do próprio app.
   - Detalha explicitamente: quais dados são coletados, por quê, como são usados, com quem são compartilhados (incluindo qualquer IA de terceiros, ex. Claude/Anthropic).
2. **Formulário "Segurança dos dados" (Data Safety)**
   - Preenchido mesmo se o app não coletar dados (declarar isso explicitamente).
   - Declarações batem com o comportamento real do app (o Google audita isso e pune divergência).
   - Localização: diferenciar "aproximada" de "exata" corretamente.
3. **Permissões**
   - Solicitar apenas o mínimo necessário para a funcionalidade principal.
   - Cada permissão sensível (localização, câmera, SMS, contatos) precisa de justificativa clara e, se aplicável, consentimento em destaque (prominent disclosure) dentro do app antes do uso.
4. **Classificação de Conteúdo**
   - Questionário de rating preenchido — app sem classificação será removido/barrado.
5. **Verificação de Desenvolvedor**
   - App registrado em Play Console > Android Developer Verification.
6. **Target API Level**
   - `targetSdkVersion` no nível mínimo exigido pelo Google para o ano corrente (prazo 31/08 de cada ano).
7. **Dados sensíveis (LGPD/CPF/saúde/financeiro)**
   - Nunca coletar mais do que o necessário.
   - Criptografia em trânsito e, quando aplicável, em repouso (ex.: CPF encriptado no MontaMovel).
   - Fluxo de exclusão de conta e dados acessível via link (obrigatório para apps com criação de conta).

---

## 3. Regra específica RhoneyInc: IA de terceiros (Claude/Anthropic) dentro dos apps

Como vários produtos (FinWise, MontaMovel, e potencialmente outros) embutem agentes de IA via API da Anthropic, siga sempre este padrão ao planejar ou codar qualquer feature de IA:

- **Divulgação explícita**: a Política de Privacidade do app deve declarar que mensagens/dados enviados ao chat de IA são processados por um provedor terceiro (Anthropic/Claude), e resumir que tipo de dado trafega (ex.: texto da conversa, dados financeiros do módulo, etc.).
- **Consentimento em destaque**: antes do primeiro uso do recurso de IA, mostrar um aviso/opt-in claro — não pode ser silencioso nem enterrado em termos de uso genéricos.
- **Minimização de dados enviados à IA**: nunca enviar CPF, dados de saúde ou dados financeiros completos ao prompt da IA se não for estritamente necessário para a funcionalidade. Prefira enviar apenas o necessário (ex.: categoria de gasto, não o extrato bancário inteiro).
- **Sem venda/uso indevido**: nenhum dado do usuário pode ser reaproveitado para treinar modelos ou repassado a terceiros além do necessário para a resposta da IA.
- **Aplicar isso retroativamente**: ao revisar FinWise e MontaMovel, adicionar uma seção específica "Uso de Inteligência Artificial" na Política de Privacidade de cada um, caso ainda não exista.

---

## 4. Mapeamento por produto RhoneyInc

- **MeuPet** — TWA/Bubblewrap publicado. Prioridade: target API level, verificação de desenvolvedor (em andamento), declaração correta de localização aproximada/exata (GPS + IP fallback), classificação de conteúdo.
- **FinWise** — dados financeiros sensíveis + agentes de IA por módulo. Prioridade: seção 3 (IA de terceiros), criptografia de dados financeiros, Política de Privacidade específica para CVM/LGPD já mapeada anteriormente.
- **MontaMovel** — CPF criptografado, chat de IA flutuante por módulo. Prioridade: seção 3 (IA), dados sensíveis (CPF), permissões mínimas para módulo de rotas/geolocalização.
- **AmaVida** — público idoso, dados de saúde (CNES/DATASUS), onboarding com cuidador. Prioridade: dados de saúde exigem tratamento redobrado (não são "app de saúde" regulado, mas expõe conteúdo de saúde — checar política de "Conteúdo e serviços relacionados à saúde"), consentimento do cuidador, dados sensíveis do idoso.
- **MenuFlex** — dados de comerciantes/pedidos, geolocalização por proximidade (100/300/500m). Prioridade: declaração de localização, dados mínimos de estabelecimento.
- **Até Passar** — dados de estudo/cadastro de candidatos a concurso. Prioridade: baixa sensibilidade, mas ainda exige Política de Privacidade e Data Safety padrão.
- **Football Results API** — não é app de usuário final (API), políticas de Play Console não se aplicam diretamente, mas se algum consumidor da API publicar app usando os dados, a responsabilidade de disclosure é do app consumidor.

---

## 5. Fontes oficiais (consultar sempre que uma feature nova tocar em dados sensíveis, IA ou permissões)

- Aviso de políticas 15/07/2026: https://support.google.com/googleplay/android-developer/answer/17134731
- Prazos de políticas: https://support.google.com/googleplay/android-developer/table/12921780
- Dados do Usuário: https://support.google.com/googleplay/android-developer/answer/10144311
- Segurança dos Dados (Data Safety): https://support.google.com/googleplay/android-developer/answer/10787469
- Classificações de Conteúdo: https://support.google.com/googleplay/android-developer/answer/9898843
- Nível de API exigido: https://support.google.com/googleplay/android-developer/answer/11926878
- Verificação de desenvolvedor: https://support.google.com/googleplay/android-developer/answer/17125096
- Permissões de SMS e Registro de Chamadas: https://support.google.com/googleplay/android-developer/answer/10208820
- Conteúdo e serviços de saúde: https://support.google.com/googleplay/android-developer/answer/16679511

**Nota de manutenção**: o Google atualiza políticas com frequência. Ao iniciar qualquer novo projeto ou grande feature, faça uma checagem rápida (web search) se houve novo "Aviso sobre políticas" desde a última revisão desta skill, e atualize a seção 1 se necessário.
