# Drive Club — landing page (V2)

Página de vendas de arquivo único da comunidade fechada do [@driveitlikestoleit](https://instagram.com/driveitlikestoleit).

Sem build, sem framework. Sobe direto na Vercel ou no Netlify (sem build command, output `/`).

Dependências externas: GSAP 3 + ScrollTrigger + MotionPathPlugin (cdnjs), Lenis (jsDelivr) e 3 famílias do Google Fonts (Archivo variable, Space Grotesk, Martian Mono). Ícones são SVG desenhados no próprio arquivo — nenhuma biblioteca de ícones.

## O que trocar

| O quê | Onde no `index.html` |
|---|---|
| `{{LINK_CHECKOUT}}` | 2 ocorrências: botão do header e CTA da seção 05 |
| `{{PRECO}}` | `data-valor="{{PRECO}}"` no `#odo` — o odômetro lê os dígitos sozinho (mostra 47 enquanto não trocar) |
| `{{VAGAS}}` | microcopy ao lado do CTA do hero |
| Fotos/vídeos | 4 `data-asset` na seção 02 (16:9, 16:9, 4:5, 21:9) |
| Domínio e OG (1200×630) | comentários `<!-- TROCAR -->` no `<head>` |

Os placeholders de mídia são frames de câmera com timecode correndo — trocar por `<img>`/`<video>` mantendo a proporção não gera CLS.

## Parâmetros de ajuste rápido

No objeto `CFG` no topo do `<script>`: `driftMax` (graus de derrapagem), `driftCurva`, `driftVeloc`, `driftInercia` (menor = mais contraesterço), `carroInercia`, `skewMax`, `blurMax`, `marqueeBase`, `marqueeGanho`, `fumacaLimite`.

Cores: as três estão em `:root` (`--preto`, `--branco`, `--laranja`). O tema de cada seção vem das classes `.t-branco` / `.t-laranja`.
Trajeto do carro: array `base` dentro de `waypoints()` (x,y normalizados pela viewport).

## Notas de implementação

- **Carro em SVG** percorre um trajeto gerado em px conforme a viewport, com `MotionPathPlugin`. O progresso do scroll é remapeado para que as curvas fechadas caiam exatamente nas divisas entre seções.
- **Derrapagem** = curvatura do trecho + velocidade do scroll, com inércia baixa: o ângulo atrasa em relação à trajetória, e é esse atraso que lê como contraesterço na saída da curva. As rodas dianteiras giram no sentido contrário.
- **Marcas de pneu** ficam na tela (latch no progresso máximo) em duas camadas de blend — `multiply` marca no branco/laranja, `screen` marca no preto.
- `prefers-reduced-motion` ou CDN fora do ar → classe `.rm`: sem carro, sem marcas, sem blur, sem skew; barra de progresso e conta-giros continuam funcionando por scroll passivo.
