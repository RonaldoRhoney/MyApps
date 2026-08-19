---
description: Regra transversal RhoneyInc — nenhuma integração externa pode gerar cobrança automática sem aprovação humana explícita. "Free tier" não é sinônimo de "custo zero". Use antes de integrar qualquer API/serviço externo em qualquer produto RhoneyInc, e ao avaliar se uma fonte de dado é segura pra ativar sem supervisão.
---

# Zero-Cost API First

Regra institucional do Ronaldo (2026-08-16, formalizada a partir de uma decisão tomada no Voa Radar): **"custo R$ 0" é restrição arquitetural, não preferência.** Nenhuma integração pode ficar em produção com risco de cobrança automática — nem "provavelmente não vai passar da quota", nem "é só um centavo por chamada". Se existe possibilidade de cobrança sem aprovação humana no meio, a integração está bloqueada até essa aprovação existir.

## Princípio

> "Se uma API gratuita puder gerar uma cobrança inesperada, ela não é considerada gratuita para a RhoneyInc."

"Free tier" **não** significa automaticamente "custo zero". Uma API só é `ZERO_COST` quando não existe nenhuma possibilidade de cobrança automática — nem por overage, nem por upgrade implícito de plano, nem por cartão cadastrado "só por precaução".

## Ordem de preferência (sempre nessa ordem, antes de considerar API paga)

1. Dados públicos governamentais (dados.gov.br, portais de agência reguladora, IBGE, etc.)
2. Open Data (licença aberta explícita, sem autenticação ou com auth gratuita sem risco de cobrança)
3. APIs públicas gratuitas (sem quota, ou quota generosa o bastante pro uso real)
4. APIs gratuitas com quota rígida (ex: YouTube Data API v3, 10.000 unidades/dia — real, sem cobrança, mas quota curta)
5. APIs em ambiente de desenvolvimento/teste **que continuem existindo de fato** — sempre confirmar que o programa não foi descontinuado antes de contar com ele (achado real: Amadeus Self-Service foi desligado em 17/jul/2026, um mês antes desta skill existir — "eu vi isso funcionando uma vez" não é verificação)
6. APIs self-hosted/open source
7. APIs comerciais **só mediante aprovação explícita** — nunca integradas "de passagem" dentro de outra tarefa

## Antes de adicionar qualquer API nova, verificar de verdade (nunca assumir)

1. Documentação oficial atual — não confiar em resumo de terceiro sem confirmar na fonte primária.
2. Pricing real (não só a página de marketing "grátis para começar").
3. Limites de quota exatos.
4. Termos de uso — inclusive restrição de uso comercial vs. não-comercial (achado real: OpenSky Network é gratuita, mas só para uso não-comercial/pesquisa).
5. Licença dos dados — "dado aberto" não é automaticamente uma licença específica; confirmar se cobre o uso pretendido.
6. Possibilidade de overage/cobrança automática.
7. Necessidade de cartão de crédito cadastrado (mesmo que "não cobra" — cartão cadastrado é sinal de risco).
8. **O provedor ainda existe/aceita novos registros** — testar a fonte de verdade (curl, fetch), nunca assumir a partir de busca genérica ou memória de treinamento desatualizada.
9. Se o dado buscado (ex: preço de passagem) é realmente o que a API entrega — achado real: OpenSky e Aviationstack são APIs de aviação reais e gratuitas, mas nenhuma das duas fornece preço de passagem, só rastreamento/status de voo. Confirmar o **conteúdo exato** do retorno antes de desenhar arquitetura em cima da suposição.

## Fail-safe

Se qualquer um destes for desconhecido — quota, preço, possibilidade de cobrança, licença — **a resposta é não integrar em produção**, documentar o que falta descobrir, e perguntar antes de assumir.

## Proibido, sem exceção

Não implementar: cartão de crédito obrigatório pro funcionamento normal; billing automático; overage automático sem trava; chamada ilimitada sem controle de quota; scraping pra contornar limite de API; engenharia reversa de API privada; bypass de paywall; bypass de autenticação; scraping de site contra os termos de uso publicados.

## Obrigatório em toda integração nova

Documentar: fornecedor, URL oficial, finalidade, limite gratuito real, licença, atribuição necessária (se houver), método de autenticação, limite de requisições, risco de cobrança, estratégia de fallback, estratégia de cache, circuit breaker.

## Métrica de classificação — `cost_status`

Toda integração externa recebe uma dessas quatro etiquetas, sempre registrada na decisão arquitetural do produto:

```text
ZERO_COST        — sem possibilidade de cobrança automática. Pode ser ativada sem aprovação extra.
FREE_WITH_LIMIT  — gratuita mas com quota real (ex: YouTube 10k/dia). Exige circuit breaker/quota
                   tracking antes de ir pra produção — nunca "confiar que não vai estourar".
PAID             — qualquer chance de cobrança. Fica BLOCKED até aprovação humana explícita do
                   Ronaldo, mesmo que o valor pareça baixo.
BLOCKED          — provedor pago sem aprovação, ou provedor descontinuado/indisponível.
```

Só `ZERO_COST` pode ser ativado automaticamente. `FREE_WITH_LIMIT` sempre precisa de proteção contra estouro (nunca degradar pra cobrança sozinho — ao atingir quota, o provider vira `DISABLED`, nunca "cobra automaticamente"). `PAID` nunca vira `ZERO_COST`/`FREE_WITH_LIMIT` por escolha do código — só por decisão humana registrada.

## Circuit breaker — obrigatório em todo provider externo

Timeout, retry limitado, rate limiting, cache, circuit breaker, rastreamento de quota. Ao atingir a quota: `provider → DISABLED`. **Nunca**: `quota → pagamento automático`.

## Falha de fonte externa nunca derruba o produto

Mesmo princípio já usado no KnowRa Scout (`KNOWRA_SCOUT.md`): se uma fonte externa está indisponível, fora de quota, alterada ou bloqueada, o produto continua funcionando com o que já tem (cache, fallback, provider secundário) — nunca "a API caiu, o produto caiu junto".

## Exemplo real que motivou esta skill (Voa Radar, 2026-08-16)

Proposta original incluía Amadeus (test env), OpenSky e Aviationstack como providers candidatos de preço de passagem. Verificação real, não suposição, achou: Amadeus Self-Service foi descontinuado em 17/jul/2026 (um mês antes desta decisão); OpenSky e Aviationstack são APIs reais e gratuitas, mas **nenhuma das duas fornece preço de passagem** — só rastreamento/status. Das fontes propostas, só a ANAC (dados abertos governamentais, download CSV direto, sem token) sobrou como viável — e mesmo essa só como referência histórica, nunca como oferta comprável em tempo real. A lição: pesquisa de segunda mão sobre API ("dizem que X é grátis e serve") não substitui checar a fonte primária de verdade antes de desenhar arquitetura em cima dela.
