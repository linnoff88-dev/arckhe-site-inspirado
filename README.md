# site-inspirado

<div align="center">
  <img src="capa/logo.svg" alt="Arckhé AI" height="48" />
  <p><strong>Arckhé AI</strong></p>
</div>


[![site-inspirado](guia/assets/banner.jpg)](https://inematds.github.io/site-inspirado/guia/)

**ðŸ‡§ðŸ‡· [PortuguÃªs](README.md) Â· ðŸ‡ºðŸ‡¸ [English](README.en.md) Â· ðŸ‡ªðŸ‡¸ [EspaÃ±ol](README.es.md)**

## O que Ã©

O site-inspirado Ã© um kit com uma skill para o Claude Code que cria sites que nÃ£o tÃªm cara de "feitos por IA". VocÃª entrega imagens de referÃªncia (do Pinterest, do convite jÃ¡ impresso, da foto da loja) e a IA faz perguntas e pesquisa antes de gerar qualquer coisa. Depois ela mede o estilo dessas imagens, gera as imagens do site no mesmo estilo pelo Codex e monta a pÃ¡gina com transiÃ§Ãµes. No fim, um teste automÃ¡tico confere se o botÃ£o principal aparece e se a pÃ¡gina funciona no celular. Para usar, vocÃª precisa do Claude Code, do Codex logado na sua assinatura, de Python e de Node.

**Site que nÃ£o tem cara de "feito por IA".** VocÃª entrega 2 a 5 imagens de referÃªncia (Pinterest, o
convite que jÃ¡ imprimiu, a foto da loja). A IA pergunta e pesquisa antes de gerar. Todas as imagens
saem no mesmo estilo das referÃªncias, a pÃ¡gina ganha transiÃ§Ãµes em cÃ³digo, e um portÃ£o confere se o
visitante ainda acha o botÃ£o antes de publicar.

[Guia completo](https://inematds.github.io/site-inspirado/guia/)

Pilotos no ar:
- **Evento** (visual, casamento fictÃ­cio em aquarela): https://inematds.github.io/site-inspirado/pilotos/evento/
- **Padaria** (negÃ³cio, botÃ£o de encomenda na 1Âª tela): https://inematds.github.io/site-inspirado/pilotos/padaria/

## O que tem aqui

| PeÃ§a | Arquivo | Faz |
|---|---|---|
| Skill | `skill/site-inspirado/SKILL.md` | os 6 passos, com portÃ£o em cada um |
| Perguntas | `skill/site-inspirado/referencias/perguntas.md` | o que perguntar antes de gerar, por tipo de site |
| Estilo medido | `skill/site-inspirado/scripts/estilo.py` | referÃªncias â†’ paleta medida, papÃ©is sugeridos com contraste, prompt de estilo |
| Imagens | `skill/site-inspirado/scripts/imagens.py` | gera no mesmo estilo pelo Codex (image_gen, assinatura, referÃªncias anexadas), uma por vez; folha de contato |
| Movimento | `skill/site-inspirado/runtime/movimento.js` | revelar na rolagem, parallax, vÃ©u de abertura, contagem; para tudo com "reduzir movimento" |
| PortÃ£o | `skill/site-inspirado/scripts/portao.js` | visual Ã— conversÃ£o: CTA, erros, peso, celular, reduzir movimento |
| Medida | `skill/site-inspirado/referencias/visual-x-conversao.md` | quanto movimento usar em site de negÃ³cio e em site visual |

## O processo, do inÃ­cio ao fim (8 passos)

ðŸ§  = sessÃ£o com a pessoa (gasta tokens) Â· âš™ï¸ = script (zero token) Â· ðŸ¤– = `claude -p` curto

| # | Passo | Comando / o que acontece | Quem |
|---|---|---|---|
| 1 | ReferÃªncias | 2 a 5 imagens em `T/refs/` (Pinterest, convite, foto da loja) | pessoa |
| 2 | Perguntas e pesquisa | a IA faz as perguntas de `perguntas.md`, pesquisa o que Ã© fato e escreve `T/BRIEF.md` com o tipo (`negocio` ou `visual`) | ðŸ§  |
| 3 | Estilo | `python3 scripts/estilo.py T/refs T/estilo.json --prompt "<estilo>"` | âš™ï¸ + ðŸ§  (o prompt) |
| 4 | Plano das imagens | `T/plano.json`: uma linha por imagem (assunto, tamanho) | ðŸ§  |
| 5 | Gerar | `python3 scripts/imagens.py T/plano.json T/estilo.json T/img --refs T/refs` e olhar `T/img/folha.jpg` | âš™ï¸ |
| 6 | PÃ¡gina | `T/index.html` desenhado para o assunto, `data-cta` no botÃ£o, `movimento.js` | ðŸ§  |
| 7 | PortÃ£o | `node scripts/portao.js T/index.html --tipo negocio --fotos T/fotos` â†’ `PASSA` | âš™ï¸ |
| 8 | Publicar | commit + push (GitHub Pages) ou o deploy do cliente | ðŸ§  |

Testes do kit: `bash tests/rodar.sh` (estilo, portÃ£o nos dois pilotos e uma pÃ¡gina ruim que tem de ser barrada).

## Instalar a skill

```bash
ln -s "$PWD/skill/site-inspirado" ~/.claude/skills/site-inspirado
cd skill/site-inspirado/scripts && npm install && npx playwright install chromium   # sÃ³ o portÃ£o precisa
```

Imagens: Codex CLI logado na assinatura (`codex exec` com `image_gen`). Reserva: `--motor local` usa um servidor
flux2-klein em `$IMG_API` (padrÃ£o `http://localhost:8000`) â€” pesado, travou a mÃ¡quina em 01/11 e 04/11/2026. VÃ­deo animado a partir das imagens (Kling, Magnific e similares) Ã© opcional e sÃ³ com
autorizaÃ§Ã£o para aquele serviÃ§o; sem ele, todo o movimento Ã© cÃ³digo.

## DiferenÃ§a para outros kits INEMA

- **site-movimento**: o topo vira um filme contÃ­nuo que anda com a rolagem.
- **filmarca**: transforma um site existente em filme da marca.
- **site-inspirado**: referÃªncia â†’ estilo medido â†’ imagens prÃ³prias â†’ pÃ¡gina com o botÃ£o Ã  vista.

LicenÃ§a MIT.

---

<div align="center">
  <img src="capa/logo.svg" alt="Arckhé AI" height="32" />
  <p><strong>Arckhé AI</strong></p>
</div>