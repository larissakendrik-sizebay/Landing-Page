# Instruções para o Claude

## Figma: onde criar as alterações

- Sempre criar cópias e alterações **dentro do link/nó que o usuário enviou**, e nunca em outra página ou fora de vista.
- Se o usuário enviar um `node-id` (ex.: `?node-id=870-6`), esse é o destino: a cópia deve ficar **dentro desse nó/seção**, ao lado do original, e não em coordenadas distantes da página.
- Ao clonar um frame, posicionar a cópia **imediatamente ao lado** do original (mesmo pai, mesmo `y`, `x` = largura do original + ~100 px), sem sobrepor outros itens.
- Se o destino não estiver claro, perguntar antes de criar.
- Ao terminar, devolver o link direto para a cópia (`?node-id=` da cópia) dentro do mesmo arquivo.

### Referência

- Arquivo da LP: `https://www.figma.com/design/eITkDttfz0x9CtWd66Yu2n/02.9.-Landing-Page`
  - LP Parceiros (original): `node-id=870-7`
  - Destino das cópias da LP Parceiros: `node-id=870-6`
- Identidade visual de referência (Redesign Site): `https://www.figma.com/design/Ed1Fk79P9lUw3tjJozOLDA/Redesign-Site` (somente leitura/referência)

## RD Station: como implementar a página

O RD Station limita o design, então as seções que ele não consegue fazer são criadas em **HTML** e coladas em blocos HTML do RD. O usuário diz qual seção quer (e envia o código/estrutura do elemento quando necessário) e o Claude gera o HTML.

- **Uma seção = um arquivo** `rd-<nome>.html` na raiz, pronto para colar em **um** bloco HTML do RD (CSS em `<style>` e, se preciso, JS em `<script>` dentro do próprio arquivo).
- **CSS geral da página**: fica no arquivo `rd-css-geral.css`, colado no campo de CSS geral do RD (sem tags `<style>`). Ele define a fonte e a animação de entrada das seções; os blocos HTML **não** devem repetir isso.
- **Fonte**: sempre **Outfit** (Google Fonts), já carregada pelo CSS geral. Nos blocos usar `font-family:var(--sbz-font)`, sem importar fonte de novo.
- **Cores e tokens**: usar as variáveis do `:root` do CSS geral (`--sbz-text`, `--sbz-blue`, `--sbz-purple`, `--sbz-bg`, ...), vindas da identidade do Redesign Site. Raio dos botões 8px, cards 22px, sombra `0 16px 44px rgba(21,80,122,.2)`.
- **Prefixo `sbz-`** em todas as classes e variáveis, para não conflitar com o CSS do RD.
- **Animação de entrada**: adicionar `class="sbz-section"` no elemento raiz de cada bloco HTML (as seções nativas do RD, `rd-section-*`, já são animadas). Variações: `sbz-from-left`, `sbz-from-right`, `sbz-zoom`, `sbz-stagger` (container que escalona filhos). Para excluir: `sbz-no-reveal`.
- **Responsivo** (media/container queries) e funcionando sem JavaScript: se o RD bloquear o script, tudo continua visível.
- **Código vindo do RD**: quando o usuário enviar o HTML de um elemento do RD, usar os IDs reais (`rd-text-*`, `rd-button-*`, ...) em vez de inventar seletores.
- Textos das seções seguem o idioma do Figma (hoje PT-BR na LP Parceiros).
- `rd-interacoes.html` e os demais `rd-*.html` anteriores usam Poppins e a identidade antiga; ao reaproveitá-los, migrar para Outfit/cores novas.
