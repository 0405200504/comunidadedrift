# Drive Club — landing page (V3)

Página de vendas de arquivo único da comunidade fechada do [@driveitlikestoleit](https://instagram.com/driveitlikestoleit).

Sem build, sem framework: `index.html` + a pasta `img/`. Sobe direto na Vercel ou no Netlify (sem build command, output `/`).

Dependências externas: GSAP 3 + ScrollTrigger (cdnjs), Lenis (jsDelivr) e 4 famílias do Google Fonts (Chakra Petch nos títulos e botões, Plus Jakarta Sans, Space Grotesk, JetBrains Mono). Ícones são SVG desenhados no próprio arquivo.

## O que trocar

| O quê | Onde no `index.html` |
|---|---|
| Link de checkout | 3 `href` com o valor fictício `https://pay.driveclub.com.br/fundador`: header, CTA da seção 05 e manifesto final |
| Preço | `data-valor` do `#odo` (hoje `39`) — o odômetro lê os dígitos sozinho; ajuste também os dois `<span>` de fallback logo abaixo |
| Vagas | número fictício `150` no microcopy ao lado do CTA do hero |
| Fotos | arquivos em `img/` (ver mapa abaixo) |
| Domínio e OG (1200×630) | comentários `<!-- TROCAR -->` no `<head>` |

### Mapa das imagens

| Arquivo | Onde aparece |
|---|---|
| `m4-neblina-noite.jpg` | quadro do hero |
| `m4-amarela-pista.jpg`, `m4-preta-rua.jpg`, `m4-espanha-vertical.jpg`, `m4-drift-pista.jpg` | grade de fotos da seção 02 (16:9, 16:9, 4:5, 21:9) |
| `m4-posto.jpg`, `m4-rua-frente.jpg`, `m4-policia-traseira.jpg`, `m4-deic-portas.jpg`, `deic-story.jpg`, `logo-graffiti.jpg` | faixa de fotos em loop, logo abaixo do hero |
| `logo-real-underground.png` | marca no centro da abertura (PNG com fundo recortado) |
| `logo-gta.png` | marca acima do manifesto final |

Os `.png` dos logos foram gerados a partir dos `.jpg` originais (fundo preto virou transparência) — os `.jpg` seguem na pasta como fonte.

## Abertura (preloader)

Antes da página aparecer, um carro entra de lado e faz donuts em volta da marca, deixando marcas de pneu e fumaça; na saída ele acelera para fora da tela e a cortina sobe revelando o hero.

- Tudo em canvas 2D puro, sem biblioteca, num `<script>` inline no topo do `<body>` — roda antes do GSAP carregar.
- Duração: `T_ENTRA` (760ms) + `T_DONUT` (2400ms, `VOLTAS = 2.25`) + `T_SAI` (620ms) + cortina (860ms). Clique, `Esc`, `Enter` ou espaço pulam.
- Sem contador de porcentagem nem leitura de telemetria: só a marca, a animação e uma barra fina de progresso.
- `prefers-reduced-motion` → a abertura nem aparece (a classe `pl-on` só é aplicada fora desse caso), e há um timeout de segurança que nunca deixa a página presa.
- Marcas de pneu ficam num canvas próprio que só esmaece (`destination-out`); a fumaça é um sprite radial pré-renderizado reaproveitado por partícula.

## Movimento e interação

- **Cursor**: nativo. A luz laranja com blur que existia aqui foi removida — `filter: blur()` + `mix-blend-mode` num elemento que se move a cada frame repintava a tela inteira e derrubava o FPS no desktop.
- **Scroll**: Lenis no desktop; revelações de cards e odômetro do preço via ScrollTrigger.
- **Faixa de fotos**: marquee CSS infinito, pausa no hover; sob `prefers-reduced-motion` vira faixa com rolagem horizontal.
- **Custo de pintura**: `backdrop-filter` ficou só no header (área pequena e fixa) — os cards e as caixas do filtro usam fundo quase opaco, que dá o mesmo resultado sem recompor o que está atrás a cada scroll.
- Não existe mais carro percorrendo a página conforme o scroll (removido na V3 junto do HUD de RPM).

Cores em `:root` (`--bg`, `--orange`, escala de cinzas). Tipografia display/body/mono também em `:root`.

## Direção

Referência: corrida de rua à noite — asfalto, poste de sódio e neon frio, não painel de simulador.

- **Tipografia**: Chakra Petch 700 itálico (angular, com itálico de verdade — nada de `skew`) nos títulos, número dos cards, preço, marca e botões; Space Grotesk no texto corrido; JetBrains Mono nos rótulos. Nas duas primeiras linhas do hero o texto é vazado (`-webkit-text-stroke`) e vira sólido na virada — de fora se olha, de dentro se anda.
- **Cor**: laranja `--orange` como marca e um frio de poste `--neon` (#2FB4FF) em doses pequenas (etiqueta de vagas, lista positiva, brilho interno dos cards). Fundo com duas poças de luz e vinheta.
- **Textura**: granulado de filme em duas camadas — uma parada no fundo, outra animada por cima de tudo (`.bg-grao`, desligada em `prefers-reduced-motion`) — via `feTurbulence` embutido em data URI, sem imagem externa.
- **Formas**: cantos praticamente retos (`--radius-*` entre 2 e 6px), botões com canto chanfrado no lugar de pílula, divisor de seção em zebrado diagonal e faixa de fotos inclinada 1,1°.
- **Fotos**: contraste alto e brilho baixo, com estouro quente no topo e sombra fria embaixo; voltam ao normal no hover.
- **Marcas de pneu** (`.rastro`): SVG inline de duas bandas paralelas com textura de banda de rodagem e ponta que some no gradiente. Em três lugares — saindo do hero (branco, quase imperceptível), cruzando a seção clara (escuro, é o grafismo principal) e atrás do fecho (espelhado).
- **Seção clara** (`.sec--claro`, o "para quem é"): papel/concreto `#E7E3DC` com granulado em `multiply`, zebrado preto nas duas bordas, caixas brancas com sombra dura e o veredito num bloco preto. É a única quebra de luz da página — serve de respiro entre o miolo e a oferta.
- **Cara de jogo**: cantoneiras de mira que aparecem no hover dos cards e das fotos, e um brilho que varre os botões — feedback de HUD, sem inventar dado na tela.

Nada de dado inventado na interface: sem "REC 4K", timecode correndo, ficha técnica falsa no hero, status "ao vivo" no header ou depoimento sem autor. Fotos aparecem como fotos; rótulos de seção são palavras, não códigos (`02 // 06`). Títulos usam cor sólida — sem gradiente em texto nem itálico postiço.
