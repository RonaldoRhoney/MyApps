# MyApps — RhoneyInc

Repositório-índice do workspace de produtos RhoneyInc. Cada produto abaixo é um
repositório próprio, independente, referenciado aqui como **git submodule** —
clonar este repositório com `--recurse-submodules` traz tudo de uma vez, mas
cada produto continua sendo versionado, deployado e mantido no seu próprio
repositório, sem mudança de fluxo.

```bash
git clone --recurse-submodules git@github.com:RonaldoRhoney/MyApps.git
```

## Produtos (submodules)

| Produto | Repositório |
|---|---|
| AmaVida | [RonaldoRhoney/AmeVida](https://github.com/RonaldoRhoney/AmeVida) |
| AtePssar | [RonaldoRhoney/AtePsaar](https://github.com/RonaldoRhoney/AtePsaar) |
| Controle Financeiro Pessoal (FinWise) | [RonaldoRhoney/Controle-Financeiro-Pessoal](https://github.com/RonaldoRhoney/Controle-Financeiro-Pessoal) |
| FinTra | [RonaldoRhoney/FinTra](https://github.com/RonaldoRhoney/FinTra) |
| Fit-Now | [RonaldoRhoney/Fit-Now](https://github.com/RonaldoRhoney/Fit-Now) |
| KnowRa | [RonaldoRhoney/knowra](https://github.com/RonaldoRhoney/knowra) |
| MENTAL | [RonaldoRhoney/Mental](https://github.com/RonaldoRhoney/Mental) |
| MenuFlex | [RonaldoRhoney/MenuFlex](https://github.com/RonaldoRhoney/MenuFlex) |
| MontaMovel | [RonaldoRhoney/MontaMovel](https://github.com/RonaldoRhoney/MontaMovel) |
| RhoneyInc (hub) | [RonaldoRhoney/RhoneyInc](https://github.com/RonaldoRhoney/RhoneyInc) |
| VagaLume | [RonaldoRhoney/VagaLume](https://github.com/RonaldoRhoney/VagaLume) |
| VendeFlex | [RonaldoRhoney/VendeFlex](https://github.com/RonaldoRhoney/VendeFlex) |
| VoaRadar | [RonaldoRhoney/VoaRadar](https://github.com/RonaldoRhoney/VoaRadar) |

## Exceções (fora dos submodules, de propósito)

- **MeuPet** — publicado em [RonaldoRhoney/MeuPet](https://github.com/RonaldoRhoney/MeuPet), mas rastreado dentro do repositório home-root (`/home/rhoney`) e publicado via `git subtree split`, não como checkout próprio nesta pasta. Ver `.gitignore`.
- **API-futebol, FieldPilot, PostEasy** — ainda sem repositório próprio no GitHub.

## Configuração compartilhada

`.claude/` contém as skills e agentes usados em todos os produtos (padrões
de arquitetura, políticas de segurança, checklist do Google Play, etc.) —
não é um projeto, é convenção transversal do workspace.

## Atualizando um submodule

```bash
cd VoaRadar   # ou qualquer outro produto
git pull origin main
cd ..
git add VoaRadar
git commit -m "Atualiza referência do submodule VoaRadar"
```
