# 🪵 TUNG TUNG SAHUR — TUCAS RUN

Un juego web tipo *endless runner* (al estilo Subway Surfers) donde **Tung Tung Sahur** corre
por las calles de Lima huyendo de **Pity Alvarez**, y tiene que **recojer tucas del piso
para sobrevivir** (literalmente: si se queda sin energía, Pity lo alcanza).

## Cómo jugar

Abre `index.html` en cualquier navegador moderno. **No necesita internet ni instalación.**

Si tu navegador restringe `file://`, sírvelo con cualquier servidor estático:

```bash
python3 -m http.server 8000
# luego abre http://localhost:8000
```

### Controles

| Acción | Teclado | Táctil |
|---|---|---|
| Cambiar carril | `←` `→` o `A` `D` | desliza a los lados |
| Saltar | `↑` `W` `Espacio` | desliza hacia arriba o toca |
| Deslizar | `↓` o `S` | desliza hacia abajo |
| Pausa | `P` o `Esc` | botón ❚❚ |

### Reglas

- **Hambre (barra amarilla):** baja sola con el tiempo. Cada tuca que recoges la repone.
  Si llega a cero, entras en modo hambriento: Pity acelera y te alcanza.
- **Pity Alvarez (barra roja):** siempre te sigue. Cada tuca también lo frena un poco.
  Míralo en el **retrovisor** de arriba — cuando se pone rojo es que se te viene encima.
- **Multiplicador:** encadena tucas sin fallar para subir x1 → x4.
- **Power-ups:**
  - 🏏 **Murciélago** — rompe todos los obstáculos (8 s)
  - ⚡ **Turbo** — corres mucho más rápido y Pity se queda atrás (7 s)
  - 🧲 **Imán** — atrae todas las tucas (11 s)
  - 🔇 **Silencio** — Pity se queda mudo y no avanza (6 s)

### Dificultad

- **Tranqui** — para aprender los controles
- **Normal** — el juego justo
- **Malvado** — Pity corre el doble y tienes que correr más

## Estructura

```
index.html           — interfaz, HUD, pantallas, estilos
game.js              — todo el juego (motor, personajes, IA de obstáculos)
vendor/three.min.js  — three.js r160 (MIT), local para jugar sin internet
audio/
  01-la-bebecita.mp3   — pista (loop, orden aleatorio)
  02-pala-ancha.mp3    — pista (loop, orden aleatorio)
  sfx-perfect-fart.mp3 — 30% de chance en cada salto
  sfx-persecucion.mp3  — volumen según lo cerca que esté Pity (0 → 100%)
img/
  pity-face.jpg        — tu foto, pegada fotorrealista en la cara del villano
```

Casi todo es procedural: las texturas (crudo, chapa, cemento, tierra, letreros, grafitis),
los modelos 3D, el cielo y los efectos de sonido se generan por código con Canvas y WebAudio.
Lo único que son archivos reales es el audio y tu foto.

## Detalles del villano

Pity Alvarez lleva tu foto pegada como textura sobre un parche esférico (curvo, no plano)
en el frente de la cabeza, con máscara ovalada de bordes suaves para que se funda con el
cráneo. **No hay ningún rasgo 3D en la cara** — ojos, nariz, boca y dientes se eliminaron:
la foto los reemplaza todos. Para cambiar la foto, reemplazá `img/pity-face.jpg` por otra
(idealmente cuadrada o con la cara centrada) y listo.

Los ojos amarillos, la gorra NAIK y la muela de oro que hadías antes quedaron afuera de la
versión final; si los querés de vuelta, avisame.

## Detalles técnicos

- Render 3D con three.js/WebGL, ~120 draw calls y 45k triángulos: va fluido a 60 fps.
- Contenedores con recycled pool (edificios, props, obstáculos, partículas) para no crear basura.
- Tucas, chispas y escombros usan `InstancedMesh` (una sola llamada de dibujo cada uno).
- El retrovisor es un segundo render con scissor + matriz de proyección espejada.
- Sombras dinámicas, niebla y un ciclo de 4 fases de iluminación (amanecer → día → atardecer → noche).
- El juego se auto-degrada (apaga el retrovisor) si detecta FPS bajos.

## Créditos

Homenaje de fan al meme de **Tung Tung Tung Sahur** y a **Pity Alvarez**.
Gráficos y sonido generados por procedimiento. three.js bajo licencia MIT.