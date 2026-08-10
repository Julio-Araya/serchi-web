# Reglas de marca — serchi-web

## Jerarquía de fuentes de verdad

`brand/` es el contrato de marca de este repo. Dentro de `brand/`,
`design-system.html` es upstream de todo lo demás (`tokens.css`, los SVG del
Bloom, `voice.md`, `favicon/`). Si un valor difiere entre archivos, gana el
design system; si necesitas un valor que no está ahí, decláralo como faltante
en lugar de inventarlo.

## Logo (El Bloom)

El símbolo son cuatro círculos iguales (radio 22 sobre grilla de 120) en
zig-zag — violeta `#6A48F4`, orquídea `#C05CFF`, magenta `#FF4DA6`, ámbar
`#FF8C0A` — con blend `multiply` en claro y `screen` (reverse) en oscuro.

Reglas transcritas del design system:

- **No rotar, inclinar ni reordenar los círculos.**
- **No recolorear ni cambiar el blend.**
- **No estirar el wordmark ni cambiar la tipografía.**
- **No colocar sobre fondos saturados de la paleta.** Fondos permitidos:
  Cream, blanco o Profundo sólido (con reverse/screen sobre oscuro).
- El símbolo en gradiente **solo dentro del app icon**.
- **Zona de protección:** el espacio libre mínimo alrededor del logo equivale
  al radio de un círculo (½ del símbolo) por cada lado.
- Tamaños mínimos: logo digital 20px de alto; símbolo solo, 16px.
- El símbolo solo se usa cuando «Serchi» ya está presente o el espacio es
  mínimo.
- Anatomía del lockup: Outfit 700, tracking −0.03em; símbolo = altura de
  mayúscula ×1.1; gap = ½ altura del símbolo.

## Wordmark

El wordmark es **«Serchi», con S mayúscula**. El sitio viejo usaba «se» en
minúscula: eso está muerto.

## Paleta

La paleta es **exclusivamente** la del design system (ver `brand/tokens.css`).
El repo viejo (serchi-talent-flow) usaba un violeta distinto
(`hsl(263 84% 50%)`) y un fucsia (`hsl(330 100% 46%)`): **están muertos, nunca
reintroducirlos**, ni siquiera "de referencia".

- Superficie por defecto: **Cream `#FFF7F0`, no blanco**. Blanco es para
  cards y superficies elevadas.
- El trabajo vive en un único violeta (`#6A48F4`); gradientes solo para
  entradas y celebración. Máximo 1–2 colores de fondo por pieza.

## Tipografía

Una sola familia: **Outfit** (pesos 300–800). Jerarquía por peso antes que por
tamaño. Mono: JetBrains Mono. Íconos: Lucide, stroke 2px, monocromos
(`currentColor`); nunca emoji como ícono funcional.

## Qué es "salida Lovable / plantilla de IA", y qué hacemos en su lugar

Anti-patrones detectados en la landing vieja — si el copy o el diseño se
parece a esto, está mal:

| Plantilla de IA | Serchi |
|---|---|
| «Cuatro superpoderes para transformar tu…» | Nombrar el beneficio concreto: «Cierra roles más rápido.» |
| Badge «Potenciado con IA ✨» pegado a todo | La IA aparece cuando explica un resultado, no como sticker |
| Emoji como ícono (⭐ 1–5, 🚀) | Lucide monocromo, stroke 2px |
| «Únete a los equipos que están transformando…» | Decir qué obtiene el equipo, con datos si ayudan |
| Superlativos sin sustento («#1», «revoluciona») | Afirmaciones verificables |
| Gradiente de texto en cada heading, orbes blur decorativos por defecto | Gradientes reservados a entradas y celebración |
| Jerga interna en superficie («Preparado para integración con Lovable AI») | Lenguaje del reclutador, cero jerga técnica |
| Fondo blanco genérico | Superficie Cream |
| Signos de exclamación y urgencia falsa | Confianza silenciosa |

El filtro editorial completo vive en `brand/voice.md`.
