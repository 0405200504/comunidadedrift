# Drive Club — landing page (V3)

Página de vendas de arquivo único da comunidade fechada do [@driveitlikestoleit](https://instagram.com/driveitlikestoleit).

Sem build, sem framework: `index.html` + a pasta `img/`. Sobe direto na Vercel ou no Netlify (sem build command, output `/`).

Dependências externas: GSAP 3 + ScrollTrigger (cdnjs), Lenis (jsDelivr) e 3 famílias do Google Fonts (Plus Jakarta Sans, Space Grotesk, JetBrains Mono). Ícones são SVG desenhados no próprio arquivo.

## O que trocar

| O quê | Onde no `index.html` |
|---|---|
| `{{LINK_CHECKOUT}}` | 5 ocorrências (header, hero, seção 05, manifesto, CTA fixo do mobile) |
| `{{PRECO}}` | `data-valor="{{PRECO}}"` no `#odo` — o odômetro lê os dígitos sozinho (mostra 47 enquanto não trocar) |
| `{{VAGAS}}` | microcopy ao lado do CTA do hero |
| Fotos | arquivos em `img/` (ver mapa abaixo) |
| Domínio e OG (1200×630) | comentários `<!-- TROCAR -->` no `<head>` |

### Mapa das imagens

| Arquivo | Onde aparece |
|---|---|
| `m4-neblina-noite.jpg` | quadro do hero |
| `m4-amarela-pista.jpg`, `m4-preta-rua.jpg`, `m4-espanha-vertical.jpg`, `m4-drift-pista.jpg` | 4 frames de câmera da seção 02 (16:9, 16:9, 4:5, 21:9) |
| `m4-posto.jpg`, `m4-rua-frente.jpg`, `m4-policia-traseira.jpg`, `m4-deic-portas.jpg`, `deic-story.jpg`, `logo-graffiti.jpg` | faixa de fotos em loop, logo abaixo do hero |
| `logo-real-underground.png` | marca no centro da abertura (PNG com fundo recortado) |
| `logo-gta.png` | marca acima do manifesto final |

Os `.png` dos logos foram gerados a partir dos `.jpg` originais (fundo preto virou transparência) — os `.jpg` seguem na pasta como fonte.

## Abertura (preloader)

Antes da página aparecer, um carro entra de lado e faz donuts em volta da marca, deixando marcas de pneu e fumaça; na saída ele acelera para fora da tela e a cortina sobe revelando o hero.

- Tudo em canvas 2D puro, sem biblioteca, num `<script>` inline no topo do `<body>` — roda antes do GSAP carregar.
- Duração: `T_ENTRA` (760ms) + `T_DONUT` (2400ms, `VOLTAS = 2.25`) + `T_SAI` (620ms) + cortina (860ms). Clique, `Esc`, `Enter` ou espaço pulam.
- `prefers-reduced-motion` → a abertura nem aparece (a classe `pl-on` só é aplicada fora desse caso), e há um timeout de segurança que nunca deixa a página presa.
- Marcas de pneu ficam num canvas próprio que só esmaece (`destination-out`); a fumaça é um sprite radial pré-renderizado reaproveitado por partícula.

## Movimento e interação

- **Cursor**: no desktop (`hover:hover`), o ponteiro nativo some e vira uma luz laranja com blur (arrasto suave) + um núcleo pequeno quase colado no ponteiro; a luz cresce sobre links, botões e cards.
- **Scroll**: Lenis no desktop; revelações de cards e odômetro do preço via ScrollTrigger.
- **Faixa de fotos**: marquee CSS infinito, pausa no hover; sob `prefers-reduced-motion` vira faixa com rolagem horizontal.
- Não existe mais carro percorrendo a página conforme o scroll (removido na V3 junto do HUD de RPM).

Cores em `:root` (`--bg`, `--orange`, escala de cinzas). Tipografia display/body/mono também em `:root`.
