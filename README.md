# Landing Page Sizebay (blocos RD Station)

Blocos HTML/CSS/JS independentes usados na landing page da Sizebay. Cada arquivo é colado em um **bloco HTML do RD Station** — não há build nem dependências (apenas Google Fonts: Poppins).

## Blocos

| Arquivo | O que é |
|---|---|
| `rd-hero-imagem.html` | Imagem do hero com cards "vidro" (produto, recomendação, etc.), com tamanhos S/M/L |
| `rd-numeros.html` | Bloco de números/estatísticas (5x, 50%, 12%, 40%) |
| `rd-cards-plataforma.html` | Cards "Probador Virtual / Vitrina virtual" |
| `rd-banner-alerta.html` | Banner de alerta sobre carrinhos abandonados |
| `rd-logos-marquee.html` | Faixa animada de logos de clientes |
| `rd-interacoes.html` | CSS + JS global (botões `rd-button-*`, animações de entrada). Colar **uma vez, no fim da página** |

## Como usar

1. No RD Station, adicione um bloco **HTML** na posição desejada.
2. Cole o conteúdo do arquivo correspondente.
3. Cole `rd-interacoes.html` em um único bloco ao final da página.

## Convenções

- Classes e variáveis com prefixo `sbz-` (ex.: `--sbz-text`), para não conflitar com o CSS do RD Station.
- Cores e tokens ficam em variáveis CSS no topo de cada bloco.
- Layout responsivo via media/container queries; textos atualmente em espanhol.
- Cada bloco deve funcionar sozinho (sem JS, tudo visível).

## Figma

- Arquivo da LP: [02.9. Landing Page](https://www.figma.com/design/eITkDttfz0x9CtWd66Yu2n/02.9.-Landing-Page)
- Identidade de referência: [Redesign Site](https://www.figma.com/design/Ed1Fk79P9lUw3tjJozOLDA/Redesign-Site) (somente leitura)
- Regras para o Claude (onde criar cópias no Figma): ver [`CLAUDE.md`](CLAUDE.md)
