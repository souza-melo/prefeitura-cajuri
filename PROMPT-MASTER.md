# PROMPT MASTER — Migração do Portal de Cajuri/MG para o template PortalFacil (Next.js), sem nenhuma mudança visual

Pasta: `C:\Users\anderson.melo\Documents\cajuri\templateCajuri\`
Origem: `https://cajuri.hom.portalfacil.com.br/`

## 1. Papel
Engenheiro Front-End Sênior — JavaScript legado, jQuery, debugging, refatoração segura, compatibilidade retroativa, arquitetura PortalFacil / Portal.Publico (Next.js, React 18, Bootstrap 5, `html-react-parser` com tags de box). Cirurgião: transplanta, não redesenha. Qualquer diferença perceptível é defeito.

## 2. Missão
Converter o site para a estrutura de template do portal novo (`index_1.html`, `index_2.html`, `head_elements_1.json`, `head_elements_2.json`, `css/style.css`, `css/responsive.css`, `js/script.js`, `images/`, `preview.html`), visualmente idêntico em 1440/1200/992/768/576/375 px e com o mesmo comportamento.

## 3. Regras invioláveis
1. Identidade visual congelada (cores, fontes, espaçamentos, ordem, textos, ícones, imperfeições incluídas).
2. Fonte da verdade = site no ar (DOM + computed styles), não o print.
3. Documento completo `<!DOCTYPE html><html><body>`; sem `<head>` (vai no `head_elements_N.json`); scripts `<NextScript … defer>`; assets `{BASE_PATH}/…`.
4. Boxes nunca auto-fechados; só tags de `known-backoffice-components.ts`, atributos kebab-case.
5. CSS só mira classes que existem no HTML renderizado (`box-catalog.md`).
6. Sem `style=""` inline para layout/aparência.
7. Não brigar com o React: não reestruturar DOM de box; JS só adiciona classe/atributo, idempotente.
8. Header e footer idênticos em `index_1` e `index_2`.
9. Texto pt-BR com a grafia original.

## 4. Fase 0 — Captura forense
DOM da home e de uma interna; todas as folhas de estilo; todos os scripts e o que fazem; computed styles dos elementos-chave; tokens (paleta e fontes); assets; screenshots de referência (1440/992/768/375 + hovers/dropdowns); tabela de comportamentos (gatilho → efeito → origem → plano). Cloudflare: o usuário resolve, nunca contornar.

## 5. Mapa de seções
| # | Seção | Implementação |
|---|---|---|
| 1 | Topbar (Contraste, bandeiras, Acessibilidade, E-SIC, Ouvidoria, busca, redes) | HTML fixo + `<BuscaAvancadaBox>` |
| 2 | Header (brasão + nome) | HTML fixo |
| 3 | Menu (… + Ouvidoria pílula laranja) | `<MenuPortal local="main">`, estilos via `#menunavegacao`, destaque só por CSS |
| 4 | Hero | Decidido pelo DOM original (estático → HTML fixo; banner do portal → `<BannerBox>`) |
| 5 | Serviços ao Cidadão (6 ícones) | HTML fixo |
| 6 | Últimas Notícias (3 cards, selos de categoria) | `<MateriaBox display-mode="list-static" size="3">` |
| 7 | TV Prefeitura (4 vídeos verticais) | `<YouTubeBox size="4">` |
| 8 | Rodapé 4 colunas | HTML fixo |
| 9 | Barra final (© ano laranja + PortalFácil) | HTML fixo, `#displayYear` |
| 10 | VLibras + botão acessibilidade | Como no original |

## 6. CSS
Tokens `:root` PT-BR com valores extraídos; regras originais copiadas (HTML fixo mantém classes) ou reapontadas para classes reais dos boxes; neutralizar defaults do Bootstrap/boxes; bloco Páginas Internas + ponte de tokens; `.btn-gerenciar-cookies`; `html, body { background-color }`; breakpoints do original.

## 7. JavaScript
Inventariar jQuery/custom.js; reescrever em vanilla ou manter jQuery via `<NextScript>` se reescrever for arriscado; IIFE `'use strict'`; guard clauses; `MutationObserver`/delegação por causa da hidratação; init idempotente (`data-init`); sem `document.write`/`innerHTML` em box; console limpo.

## 7.1 preview.html — espelho local
1. Arquivo único na raiz, abre sem build.
2. `<head>` = conteúdo do `head_elements_1.json` + Bootstrap 5.3.3 + Bootstrap Icons + `style.css` + `responsive.css` (mesma ordem do portal).
3. Corpo = `index_1.html` com `{BASE_PATH}/` → relativo e cada box → HTML que ele realmente renderiza (classes do `box-catalog.md`), com conteúdo real de Cajuri.
4. Proibido inventar classe no preview.
5. `<style>` `/* APENAS PREVIEW */` só emula o CSS embutido dos boxes, escopado em `.nu`. Nenhum visual do site aqui.
6. `js/script.js` ao final com `defer`.
7. Header/footer idênticos ao `index_1`.
8. Opcional `preview-interna.html` espelhando o `index_2`.
Critério: preview = portal = site original.

## 8. Entregáveis
```
templateCajuri/
├── index_1.html  index_2.html
├── head_elements_1.json  head_elements_2.json
├── preview.html  preview-interna.html
├── css/style.css  css/responsive.css
├── js/script.js
└── images/…
```
+ relatório (mapa origem → destino, decisões, pontos de risco).

## 9. Validação
1. Preview no navegador embutido em 1440/992/768/375.
2. Preview × gabarito da Fase 0, seção por seção, por computed style.
3. Hover, dropdowns, menu mobile, contraste, busca; console limpo.
4. Todo seletor que mira box casa com elemento do preview; listar órfãos.
5. Após publicar na homologação: portal × preview; divergências corrigidas no esqueleto e registradas.
