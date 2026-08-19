---
description: Todo produto RhoneyInc (MeuPet, FinWise, FitNow, MontaMovel, AmaVida, RhoneyInc hub, os próximos) segue o mesmo esqueleto de rodapé — 4 colunas, mesma ordem de seções, mesma hierarquia. Só cor/logo/textos mudam por produto.
---

# Rodapé padrão RhoneyInc

Regra do Ronaldo: o rodapé de qualquer software da RhoneyInc segue a mesma estrutura (referência viva: rodapé do MeuPet, marcado no código como "padrão FinWise" — ele já nasceu como convenção entre produtos, não é exclusivo do MeuPet). O que muda de produto pra produto é **cor, logo e texto**; a estrutura, ordem das colunas e comportamento responsivo são fixos.

## Esqueleto (HTML)

```html
<footer class="site">
  <div class="wrap">
    <div class="footer-top">
      <div class="footer-brand">
        <a href="#top" class="logo"><span class="paw">🐾</span>NomeDoProduto</a>
        <p>Frase curta do que o produto faz — feito pela RhoneyInc.</p>
        <span class="footer-tagline">Uma conta. Todos os softwares.</span>
        <div class="social-icons">
          <a href="https://github.com/RonaldoRhoney" target="_blank" rel="noopener" aria-label="GitHub">💻</a>
          <a href="https://www.instagram.com/ronaldorhoney" target="_blank" rel="noopener" aria-label="Instagram">📷</a>
          <a href="https://www.linkedin.com/in/ronaldomartinsrhoney/" target="_blank" rel="noopener" aria-label="LinkedIn">💼</a>
        </div>
      </div>
      <div class="footer-col">
        <h5>Produto</h5>
        <!-- links para as seções/âncoras internas do próprio produto -->
      </div>
      <div class="footer-col">
        <h5>RhoneyInc</h5>
        <!-- links pros produtos irmãos + "Sobre nós" -->
      </div>
      <div class="footer-col">
        <h5>Legal</h5>
        <a href="/privacidade.html">Privacidade (LGPD)</a>
        <a href="/termos.html">Termos de uso</a>
        <a href="/contato.html">Contato</a>
      </div>
    </div>
    <div class="footer-bottom">
      <span>© {ano} {Produto} — um produto RhoneyInc. Todos os direitos reservados.</span>
      <span class="holding">{produto}.rhoneyinc.com</span>
    </div>
  </div>
</footer>
```

## CSS base (troque só as cores)

```css
footer.site{ background:var(--cor-fundo-produto); color:rgba(255,248,240,0.85); padding:56px 0 28px; margin-top:40px; }
.footer-top{ display:grid; grid-template-columns:1.4fr 1fr 1fr 1fr; gap:32px; padding-bottom:36px; border-bottom:1px solid rgba(255,248,240,0.12); }
.footer-brand p{ font-size:0.85rem; color:rgba(255,248,240,0.6); margin:14px 0 18px; max-width:260px; line-height:1.5; }
.footer-tagline{ font-family:'JetBrains Mono', monospace; font-size:0.72rem; letter-spacing:0.03em; background:rgba(var(--cor-acento-rgb),0.14); color:var(--cor-acento-produto); display:inline-block; padding:6px 12px; border-radius:8px; }
.social-icons{ display:flex; gap:10px; margin-top:18px; }
.social-icons a{ width:34px; height:34px; border-radius:50%; background:rgba(255,248,240,0.08); display:grid; place-items:center; font-size:0.9rem; }
.footer-col h5{ font-family:'JetBrains Mono', monospace; font-size:0.72rem; text-transform:uppercase; letter-spacing:0.08em; color:var(--cor-acento-produto); margin-bottom:14px; }
.footer-col a{ display:block; font-size:0.86rem; color:rgba(255,248,240,0.75); margin-bottom:10px; }
.footer-col a:hover{ color:white; }
.footer-bottom{ display:flex; justify-content:space-between; align-items:center; flex-wrap:wrap; gap:12px; padding-top:22px; font-size:0.78rem; color:rgba(255,248,240,0.55); }
.footer-bottom .holding{ font-family:'JetBrains Mono', monospace; }

@media (max-width:980px){ .footer-top{ grid-template-columns:1fr 1fr; } }
@media (max-width:560px){ .footer-top{ grid-template-columns:1fr; } }
```

## O que é fixo (não muda entre produtos)

1. **Ordem e quantidade de colunas**: Marca (1.4fr) → Produto → RhoneyInc → Legal (1fr cada).
2. **Coluna "RhoneyInc"**: sempre lista os produtos irmãos (cross-sell do ecossistema) + "Sobre nós" — nunca fica de fora, mesmo em produto novo com poucos links próprios.
3. **Coluna "Legal"**: sempre Privacidade (LGPD) / Termos de uso / Contato, nessa ordem.
4. **`.footer-tagline`**: a frase "Uma conta. Todos os softwares." (ou equivalente) é do ecossistema, não do produto — mantém o texto igual em todos (ver [[rhoneyinc_hub_sso_plan]] sobre a conta central).
5. **`.social-icons`**: sempre os mesmos 3 links pessoais do Ronaldo, iguais em todos os produtos (não é social do produto, é do ecossistema/autor) — GitHub (`https://github.com/RonaldoRhoney`), Instagram (`https://www.instagram.com/ronaldorhoney`), LinkedIn (`https://www.linkedin.com/in/ronaldomartinsrhoney/`). Sempre com `target="_blank" rel="noopener"`.
6. **`.footer-bottom`**: copyright + `{produto}.rhoneyinc.com` em monospace (subdomínio, não `rhoneyinc.com/{produto}` — ver correção feita no Voa Radar em 2026-08-12, o esqueleto antigo tinha o padrão errado).
7. **Breakpoints**: 980px quebra pra 2 colunas, 560px quebra pra 1 coluna.

## O que muda por produto (identidade própria)

1. **Cor de fundo do rodapé** (`--cor-fundo-produto`) = a cor de base/escura já usada no resto do produto (ex: `var(--teal)` no MeuPet).
2. **Cor de acento** (`--cor-acento-produto`, usada nos `h5` e na tag) = o accent color já estabelecido do produto (ex: `var(--mustard)` no MeuPet), não a cor da RhoneyInc.
3. **Logo/ícone e nome** na `.footer-brand`.
4. **Texto da `.footer-brand p`**: 1 frase resumindo o que aquele produto faz + "feito pela RhoneyInc".
5. **Links da coluna "Produto"**: âncoras/seções específicas daquele app.

## Como aplicar

Ao criar ou revisar o rodapé de qualquer produto RhoneyInc (novo ou existente):
1. Copie o esqueleto acima.
2. Troque só os pontos da seção "O que muda por produto".
3. Não invente uma estrutura de colunas diferente, não remova a coluna RhoneyInc nem a Legal, mesmo se o produto for pequeno — isso é o que torna o rodapé reconhecível como parte do ecossistema RhoneyInc.

## Regra ativa: nenhum produto fica de fora

Confirmado pelo Ronaldo em 2026-07-18: **este padrão é obrigatório em todos os produtos, sem exceção** — não é só uma referência opcional pro MeuPet. Um app não seguir o esqueleto (mesmo que só com um rodapé mínimo de "©... todos os direitos reservados") é considerado um bug de consistência de marca, não uma escolha de design válida.

**Sempre que for tocar em qualquer produto RhoneyInc** (nova feature, revisão, ou simplesmente de passagem), vale checar rapidamente se o rodapé dele já segue o padrão — não é preciso esperar um pedido explícito.

### Status por produto (última checagem: 2026-07-18)

| Produto | Local do rodapé | Status |
|---|---|---|
| MeuPet | `index.html` + `meupet.html` (gêmeos) | ✅ Segue o padrão — referência viva |
| RhoneyInc hub | `index.html` | ✅ Segue o padrão (colunas: Marca/Produtos/Empresa/Legal) |
| FinWise (Controle-Financeiro-Pessoal) | `src/routes/__root.tsx`, função `Footer()` | ✅ Reconstruído no padrão em 2026-07-18 |
| FitNow (Fit-Now) | `src/components/BrandFooter.tsx` | ✅ Reconstruído no padrão em 2026-07-18 |
| MontaMovel (painel / portal-cliente / montador-pwa) | — | ❌ Nenhum rodapé implementado ainda em nenhum dos 3 sub-apps |
| AmaVida | `amavida-preview.html` | ❌ Só tem rodapé de documento de planejamento/preview, sem estrutura de produto real |
| MenuFlex | `src/components/Footer.tsx` | ✅ Segue o padrão |
| VagaLume | `src/components/Footer.tsx` | ✅ Segue o padrão (criado 2026-07-25) |
| API Futebol | `portal_referencia.html` | ✅ Mockup de referência já no padrão; portal React de verdade ainda não construído |

Atualize esta tabela sempre que corrigir o rodapé de um produto.
