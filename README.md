ï»¿# site-inspirado

<div align="center">
  <img src="capa/logo.svg" alt="Arckhé AI" height="48" />
  <p><strong>Arckhé AI</strong></p>
</div>


[![site-inspirado](guia/assets/banner.jpg)](https://inematds.github.io/site-inspirado/guia/)

**ê°Å¸â¡Â§ê°Å¸â¡Â· [PortuguêÂªs](README.md) êÂ· ê°Å¸â¡Âºê°Å¸â¡Â¸ [English](README.en.md) êÂ· ê°Å¸â¡Âªê°Å¸â¡Â¸ [EspaêÂ±ol](README.es.md)**

## O que êÂ©

O site-inspirado êÂ© um kit com uma skill para o Claude Code que cria sites que nêÂ£o têÂªm cara de "feitos por IA". VocêÂª entrega imagens de referêÂªncia (do Pinterest, do convite jêÂ¡ impresso, da foto da loja) e a IA faz perguntas e pesquisa antes de gerar qualquer coisa. Depois ela mede o estilo dessas imagens, gera as imagens do site no mesmo estilo pelo Codex e monta a pêÂ¡gina com transiêÂ§êÂµes. No fim, um teste automêÂ¡tico confere se o botêÂ£o principal aparece e se a pêÂ¡gina funciona no celular. Para usar, vocêÂª precisa do Claude Code, do Codex logado na sua assinatura, de Python e de Node.

**Site que nêÂ£o tem cara de "feito por IA".** VocêÂª entrega 2 a 5 imagens de referêÂªncia (Pinterest, o
convite que jêÂ¡ imprimiu, a foto da loja). A IA pergunta e pesquisa antes de gerar. Todas as imagens
saem no mesmo estilo das referêÂªncias, a pêÂ¡gina ganha transiêÂ§êÂµes em cêÂ³digo, e um portêÂ£o confere se o
visitante ainda acha o botêÂ£o antes de publicar.

[Guia completo](https://inematds.github.io/site-inspirado/guia/)

Pilotos no ar:
- **Evento** (visual, casamento fictêÂ­cio em aquarela): https://inematds.github.io/site-inspirado/pilotos/evento/
- **Padaria** (negêÂ³cio, botêÂ£o de encomenda na 1êÂª tela): https://inematds.github.io/site-inspirado/pilotos/padaria/

## O que tem aqui

| PeêÂ§a | Arquivo | Faz |
|---|---|---|
| Skill | `skill/site-inspirado/SKILL.md` | os 6 passos, com portêÂ£o em cada um |
| Perguntas | `skill/site-inspirado/referencias/perguntas.md` | o que perguntar antes de gerar, por tipo de site |
| Estilo medido | `skill/site-inspirado/scripts/estilo.py` | referêÂªncias ê¢â â paleta medida, papêÂ©is sugeridos com contraste, prompt de estilo |
| Imagens | `skill/site-inspirado/scripts/imagens.py` | gera no mesmo estilo pelo Codex (image_gen, assinatura, referêÂªncias anexadas), uma por vez; folha de contato |
| Movimento | `skill/site-inspirado/runtime/movimento.js` | revelar na rolagem, parallax, vêÂ©u de abertura, contagem; para tudo com "reduzir movimento" |
| PortêÂ£o | `skill/site-inspirado/scripts/portao.js` | visual êâ conversêÂ£o: CTA, erros, peso, celular, reduzir movimento |
| Medida | `skill/site-inspirado/referencias/visual-x-conversao.md` | quanto movimento usar em site de negêÂ³cio e em site visual |

## O processo, do inêÂ­cio ao fim (8 passos)

ê°Å¸Â§Â  = sessêÂ£o com a pessoa (gasta tokens) êÂ· ê¢Å¡â¢ê¯Â¸Â = script (zero token) êÂ· ê°Å¸Â¤â = `claude -p` curto

| # | Passo | Comando / o que acontece | Quem |
|---|---|---|---|
| 1 | ReferêÂªncias | 2 a 5 imagens em `T/refs/` (Pinterest, convite, foto da loja) | pessoa |
| 2 | Perguntas e pesquisa | a IA faz as perguntas de `perguntas.md`, pesquisa o que êÂ© fato e escreve `T/BRIEF.md` com o tipo (`negocio` ou `visual`) | ê°Å¸Â§Â  |
| 3 | Estilo | `python3 scripts/estilo.py T/refs T/estilo.json --prompt "<estilo>"` | ê¢Å¡â¢ê¯Â¸Â + ê°Å¸Â§Â  (o prompt) |
| 4 | Plano das imagens | `T/plano.json`: uma linha por imagem (assunto, tamanho) | ê°Å¸Â§Â  |
| 5 | Gerar | `python3 scripts/imagens.py T/plano.json T/estilo.json T/img --refs T/refs` e olhar `T/img/folha.jpg` | ê¢Å¡â¢ê¯Â¸Â |
| 6 | PêÂ¡gina | `T/index.html` desenhado para o assunto, `data-cta` no botêÂ£o, `movimento.js` | ê°Å¸Â§Â  |
| 7 | PortêÂ£o | `node scripts/portao.js T/index.html --tipo negocio --fotos T/fotos` ê¢â â `PASSA` | ê¢Å¡â¢ê¯Â¸Â |
| 8 | Publicar | commit + push (GitHub Pages) ou o deploy do cliente | ê°Å¸Â§Â  |

Testes do kit: `bash tests/rodar.sh` (estilo, portêÂ£o nos dois pilotos e uma pêÂ¡gina ruim que tem de ser barrada).

## Instalar a skill

```bash
ln -s "$PWD/skill/site-inspirado" ~/.claude/skills/site-inspirado
cd skill/site-inspirado/scripts && npm install && npx playwright install chromium   # sêÂ³ o portêÂ£o precisa
```

Imagens: Codex CLI logado na assinatura (`codex exec` com `image_gen`). Reserva: `--motor local` usa um servidor
flux2-klein em `$IMG_API` (padrêÂ£o `http://localhost:8000`) ê¢â¬â pesado, travou a mêÂ¡quina em 01/11 e 04/11/2026. VêÂ­deo animado a partir das imagens (Kling, Magnific e similares) êÂ© opcional e sêÂ³ com
autorizaêÂ§êÂ£o para aquele serviêÂ§o; sem ele, todo o movimento êÂ© cêÂ³digo.

## DiferenêÂ§a para outros kits INEMA

- **site-movimento**: o topo vira um filme contêÂ­nuo que anda com a rolagem.
- **filmarca**: transforma um site existente em filme da marca.
- **site-inspirado**: referêÂªncia ê¢â â estilo medido ê¢â â imagens prêÂ³prias ê¢â â pêÂ¡gina com o botêÂ£o êÂ  vista.

LicenêÂ§a MIT.

---

<div align="center">
  <img src="capa/logo.svg" alt="Arckhé AI" height="32" />
  <p><strong>Arckhé AI</strong></p>
</div>