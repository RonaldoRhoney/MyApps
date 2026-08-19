---
description: Antes de executar um brief técnico detalhado, confira as premissas contra o código/banco real — vários pedidos chegaram com afirmações erradas sobre o estado atual.
---

# Verificar premissas antes de executar

Quando receber um brief técnico detalhado (principalmente colado de outro lugar, com nomes de tabela/coluna/comportamento específicos):

1. Grep/leia o código e o schema relevantes ANTES de aceitar as afirmações do brief como verdade.
2. Se algo não bater (tabela sem aquela coluna, feature que já existe, comportamento diferente do descrito), pare e avise antes de programar em cima da premissa errada.
3. Só prossiga a implementação depois de confirmar o que é real, ajustando o plano pro que o código realmente permite.

Já aconteceu várias vezes no MeuPet: "Supabase não está integrado" (estava), "target_type aceita 'app'" (constraint não aceitava), "comentários são mockados" (já eram reais e melhores que o pedido). Confiar cegamente no brief custou retrabalho e quase gerou regressão.
