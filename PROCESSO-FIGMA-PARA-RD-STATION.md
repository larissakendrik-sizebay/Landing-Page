# Do Figma para um bloco HTML no RD Station

Passo a passo que usamos neste repositório para transformar um frame do Figma em um bloco HTML pronto para colar no RD Station. Exemplos prontos: `rd-hero-imagem.html`, `rd-hero-imagem-puffer.html`, `rd-banner-alerta.html`, `rd-cards-plataforma.html`.

## 1. Receber o link do Figma

- Peça o link do **frame específico** (com `node-id` na URL).
- Da URL `figma.com/design/:fileKey/:nome?node-id=935-295` tiramos `fileKey` e `nodeId` (trocando `-` por `:`, ou seja, `935:295`).

## 2. Ler o design

1. Criar a sessão do arquivo no Figma (`create_session`).
2. Carregar a skill `figma-design-to-code` e chamar `get_design_context` com o `nodeId`. Ele devolve código de referência (React + Tailwind) e uma captura do design.
3. Esse código é só **referência**: não vai direto para o RD Station.
4. A captura é o alvo visual. O resultado final tem que ficar igual a ela.

## 3. Converter para HTML + CSS puro

O RD Station aceita um bloco HTML único, então tudo vai junto no mesmo arquivo:

- Um `<style>`, o HTML e, se precisar, um `<script>` no final. Sem Tailwind, sem React, sem arquivos externos.
- Fontes pelo Google Fonts (`<link>` no topo do arquivo).
- Prefixo `sbz-` em todas as classes para não colidir com o CSS do RD Station (ex.: `sbz-pf-glass`).
- Cada bloco tem um nome próprio de prefixo (`sbz-hero`, `sbz-pf`, `sbz-alert`) e um `id` único se usar JavaScript.
- Comentário no topo do arquivo: nome do bloco e "colar em um bloco HTML do RD Station".
- Comentários em português e seguindo o estilo dos outros arquivos do repositório.

### Escala proporcional (blocos com cards sobre imagem)

- Medida de referência: o tamanho do frame no Figma (ex.: 599 × 760).
- `--u: calc(100cqw / 599)` e `container-type:inline-size` no contêiner, com `aspect-ratio`. Todas as medidas viram `calc(var(--u)*N)`, usando o valor em px do Figma.
- Assim o bloco encolhe junto com a largura da coluna do RD Station, em vez de depender só da largura da tela.
- Posição dos cards: `position:absolute` com `left`/`top` iguais aos do Figma.
- Ordem de camadas (`z-index`): respeitar a ordem do Figma. Quem está atrás da foto no Figma fica com `z-index:0`, a foto com `1` e quem está na frente com `2`.

### Layouts normais (banners, cards)

- Usar flex/grid e `container query` (`@container (max-width:700px)`) para o mobile, mais um `@media (max-width:768px)` de reserva.
- Margens externas do bloco ficam **dentro** dele (`display:flow-root` no contêiner), porque o RD Station corta margens que ficam fora.

### Efeito "vidro" dos cards

`background:rgba(255,255,255,.7)`, `backdrop-filter:blur(...)`, sombra suave e raio igual ao do Figma.

### Elementos simples viram CSS

Em vez de exportar SVG, desenhamos em CSS o que é simples: barras de slider, linhas "esqueleto", campos de texto, chips. Fica mais leve e anima melhor.

### Animações (opcional)

Entrada dos cards ao aparecer na tela (`IntersectionObserver`), números que contam até o valor, sliders que preenchem e elevação no hover. O bloco precisa funcionar sem JavaScript e respeitar `prefers-reduced-motion`.

## 4. Imagens

- O Figma gera links temporários (7 dias) e o ambiente do Claude Code pode bloquear o host `www.figma.com`. **Nunca deixar link do Figma no HTML final.**
- Fluxo que funcionou:
  1. Exportar as imagens do Figma (ou deixar o Claude baixar, se o host estiver liberado).
  2. Subir as imagens no RD Station (gerenciador de arquivos).
  3. Copiar os links gerados (`https://d335luupugsy2.cloudfront.net/cms/files/...`) e colar nos `src` do HTML.
- Se o host `www.figma.com` estiver bloqueado: nas configurações do ambiente, em **Network access**, escolher um nível mais amplo ou adicionar `www.figma.com` em Allowed domains.
- Foto de modelo: usar `object-fit:contain` e `object-position:center bottom` para não cortar a imagem. Se a foto ainda aparecer cortada, o arquivo foi exportado cortado: reexportar do Figma com a imagem inteira.
- Sempre preencher o `alt` com uma descrição curta da imagem. Imagens só decorativas (esqueletos) ficam com `aria-hidden="true"`.

## 5. Idioma

Os textos seguem o Figma. Alguns blocos têm versão em espanhol (ex.: `rd-hero-imagem.html`, `rd-banner-alerta.html`). Confirmar com quem pede qual idioma usar.

## 6. Conferir antes de entregar

- Comparar o resultado com a captura do Figma: posições, tamanhos, fontes, cores, ordem das camadas.
- Testar em largura de desktop e de celular.
- Confirmar que não sobrou link `figma.com/api/mcp/asset/...` no arquivo (`grep -n "figma.com" arquivo.html`).
- Cada imagem aparece no lugar certo, inteira e sem distorcer.

## 7. Entregar

- Um arquivo `rd-<nome-do-bloco>.html` na raiz do repositório.
- Commit com mensagem descritiva, push para a branch da tarefa. Só abrir pull request se for pedido.
- Colar o conteúdo do arquivo em um bloco HTML do RD Station.

## Ajustes que já fizemos e funcionam como padrão

- Banner de alerta: imagem acima do texto em contêineres pequenos; 40px de margem inferior só no mobile.
- Cards da plataforma: 40px de margem inferior dentro do bloco.
- Hero com cards: card do produto atrás da foto, medidas na frente.

## Animação de entrada na página inteira

Em vez de animar bloco por bloco, usamos **um único script** que faz tudo aparecer (fade + subida de 28px) quando entra na tela: `rd-animacao-entrada.html`.

**Instalação (uma vez por página):** no RD, abra **Edição Avançada > Javascript em BODY**, cole o bloco `<script>` do arquivo e clique em **Aplicar**. Não precisa de CSS. A edição avançada não roda durante a edição: conferir no **Pré-visualizar**.

**O que anima, sem precisar de código por elemento:**
- Componentes nativos do RD, pelos ids `rd-text-`, `rd-title-`, `rd-image-`, `rd-video-` e `rd-button-`.
- `rd-box-` só quando tem fundo, borda ou sombra (viram cards, entram inteiros). Caixa só de layout não anima: animamos o que há dentro.
- Os blocos HTML da Sizebay, com cada card ou etapa entrando um depois do outro (`.sbz-nd__card`, `.sbz-bn__card`, `.sbz-pf3__card`, `.sbz-sol__card`, `.sbz-ps__step`, entre outros).

**O que fica de fora:** links e botões (`a`, `button`), formulário, cabeçalho e rodapé, elementos fixos, o hero com a foto (`.sbz-pf`, `.sbz-hero`, que já têm animação própria) e qualquer elemento com a classe `sbz-nofx` (no RD: campo de classe CSS do elemento).

**Segurança:** com "reduzir animações" ligado, ou se o script falhar, nada fica escondido. Se algum elemento não for detectado pelo navegador, o script o revela pela posição na tela.

**Novo bloco HTML com cards ou etapas:** para os cards entrarem um a um, adicionar o seletor da classe do card nas duas listas do script (`COMP` e a lista do filtro `o.matches(...)`). Blocos sem isso entram pelos ids do RD ou ficam sem animação de entrada.

**Atenção:** o `rd-interacoes.html` também faz entrada nos mesmos componentes. Usar só um dos dois na mesma página.

## Ajustes direto no editor do RD (CSS)

Quando a seção ou o botão foi criado direto no RD, não criamos HTML: damos o acabamento por CSS, colado em **Edição Avançada > aba CSS** (só o conteúdo, sem tag `<style>`). Depois de **Aplicar**, conferir no **Pré-visualizar**, porque a edição avançada não aparece durante a edição.

1. Pegar o **ID** do elemento no RD (painel "Dados do elemento": ex. `#rd-box-mvs6dotd`) e a classe (ex. `.rd-box`, `.rd-button`).
2. Ler o frame no Figma para ver as medidas do efeito (cor, opacidade, raio).
3. Escrever o CSS mirando o ID (um elemento) ou a classe (todos do tipo). Usar `!important` onde o RD já define o mesmo estilo.
4. Testar numa página de exemplo e enviar o arquivo `.css` clicável.

Exemplos prontos:
- `rd-glass-cta.css`: efeito de vidro no card do CTA final (`#rd-box-mvs6dotd`): fundo branco 80%, `backdrop-filter` de desfoque, borda clara, raio de 22px e sombra. O desfoque só aparece se o card for translúcido. Há um fundo mais opaco de reserva para navegadores sem suporte.
- `rd-botoes-hover.css`: hover **suave** em todos os botões `.rd-button`: o tom da cor clareia um pouco (`brightness`), surge uma sombra discreta e um brilho fraco e lento atravessa o botão. Sem crescer nem pular. A primeira versão, com subida de 4px e zoom, foi considerada forte demais.

Cuidado: o `rd-interacoes.html` também estiliza `rd-button-` (visual rosa com seta). Usar só um dos dois na mesma página.

## Blocos criados até agora

| Arquivo | O que é |
|---|---|
| `rd-hero-imagem-puffer.html` | Foto da modelo com cards de vidro (produto, ajuste, medidas) |
| `rd-numeros-destaque.html` | "1.200+ lojas" com contagem e dois cards |
| `rd-beneficios-4-colunas.html` | Quatro benefícios (4 colunas, 2 no tablet, 1 no celular) |
| `rd-cards-solucoes.html` | Provador Virtual, Try-On e Fashion Hub, com uma imagem de mockup por card |
| `rd-passos.html` | Passos 01-02-03, com o tracinho que vai surgindo (para fundo escuro) |
| `rd-perfis.html` | Perfis 01-02-03 em tons de azul |
| `rd-animacao-entrada.html` | Script de entrada ao rolar, para a página toda (JS em BODY) |
| `rd-glass-cta.css` | Efeito vidro no card do CTA final (aba CSS) |
| `rd-botoes-hover.css` | Hover suave nos botões `.rd-button` (aba CSS) |

## Lições aprendidas

- Ícones: com o host do Figma bloqueado, desenhamos os ícones simples em SVG. Para ficar idêntico, exportar os SVGs do Figma.
- Imagens complexas (mockups com fotos): exportar uma imagem por card em PNG 2x e subir no RD, em vez de recriar tudo em HTML.
- Pedido de "sem margem" ou "não esticar": manter a largura máxima do design e o bloco centralizado, a menos que a pessoa peça outra coisa.
- Sempre enviar o arquivo clicável no chat depois de criar ou alterar um bloco.
- Animações: começar suave. Movimento forte (subir, dar zoom) costuma ser rejeitado; preferir leve mudança de tom, sombra discreta e brilho lento.
- Elementos criados no RD: personalizar por CSS na aba Edição Avançada, mirando o ID ou a classe do elemento, em vez de recriar em HTML.
