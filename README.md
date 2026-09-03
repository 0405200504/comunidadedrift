# Drive Club — landing page

Página de vendas de arquivo único da comunidade fechada do [@driveitlikestoleit](https://instagram.com/driveitlikestoleit).

Sem build, sem framework. Sobe direto no Netlify (arrasta a pasta ou conecta o repo — sem build command, publish directory `/`).

Única dependência externa: GSAP 3 + ScrollTrigger via CDN, e 2 famílias do Google Fonts (Archivo variable + JetBrains Mono).

## O que trocar

| O quê | Onde |
|---|---|
| Link do checkout | `index.html` — `href="#LINK_CHECKOUT"` (é o **único** lugar; os outros CTAs apontam pra `#oferta` de propósito) |
| Preço | `index.html` — `data-valor="47"` no `#odometro` (só dígitos: o odômetro é montado a partir daí) |
| Fotos do feed (4:5, 1:1, 4:5) | `index.html` — `data-asset="feed-01.jpg"` … `feed-03.jpg` |
| Frames de bastidor (16:9, 1:1) | `index.html` — `data-asset="bastidor-01.jpg"`, `bastidor-02.jpg` |
| Domínio e imagem de OG (1200×630) | `index.html` — comentários `<!-- TROCAR -->` no `<head>` |

Os placeholders têm proporção fixa: trocar por `<img>` não gera CLS.

## Notas de implementação

- **Marca de pneu:** um `<path>` duplo (as duas rodas traseiras) que atravessa a página inteira e se desenha conforme o scroll.
- **Um efeito protagonista por seção.** Nada anima sem motivo.
- `prefers-reduced-motion: reduce` → a página inteira em estado final, sem movimento nenhum.
- Se o CDN do GSAP cair, o guard aplica a classe `.rm` e a página vira estática — nunca fica com conteúdo invisível.
