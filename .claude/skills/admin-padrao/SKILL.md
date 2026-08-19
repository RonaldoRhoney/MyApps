---
description: Todo produto RhoneyInc (MeuPet, FinWise, FitNow, MontaMovel, AmaVida, RhoneyInc hub, os próximos) tem rhoneyinc@gmail.com como conta admin padrão — não é opcional por produto.
---

# Conta admin padrão RhoneyInc

Regra do Ronaldo: em qualquer produto RhoneyInc que tenha um conceito de "admin" (role, painel administrativo, permissões elevadas), a conta **rhoneyinc@gmail.com** é sempre promovida a admin — automaticamente, não manualmente.

## Como aplicar

Ao desenhar ou revisar o schema de autenticação/perfis de qualquer produto novo ou existente:

1. Se existir uma tabela `profiles`/`users` com coluna de `role`, garanta um trigger (ex.: `handle_new_user()` no Postgres) que promove `rhoneyinc@gmail.com` a `admin` automaticamente no momento do cadastro — não importa o método de login (e-mail/senha, Google, GitHub, Apple).
2. Se a conta `rhoneyinc@gmail.com` já existir antes dessa lógica ser implementada, documente (no SETUP.md ou equivalente do produto) o comando manual pra promovê-la retroativamente.
3. Nunca deixe a promoção de admin depender de um passo manual só documentado — o trigger automático é o padrão; instruções manuais são só o fallback para contas já existentes.

## Referência viva

RhoneyInc hub (`schema.sql`, função `handle_new_user()`) já implementa isso — é a referência a copiar em produtos novos (FitNow, MontaMovel, AmaVida) quando a autenticação deles for desenhada.
