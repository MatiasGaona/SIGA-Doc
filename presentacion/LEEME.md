# Presentación de defensa — SIGA-Óptica

`presentacion.tex` es un documento **independiente** de la tesis (`../main.tex`).
No comparte `preamble.tex`: Beamer trae su propia clase y muchas de las opciones
del preámbulo de la tesis (geometry, fancyhdr, titlesec) no aplican a una
presentación. Lo que sí se replicó a mano es la **paleta institucional**
(`sigaAzul`, `sigaGris`, …) y los **estilos TikZ** (`capa`, `externo`, `entidad`,
`flechaArq`, `relacion`), para que los diagramas se vean igual que en el informe.

Las capturas se toman de `../imagenes/` — no están duplicadas.

## Cómo compilar

**Local (pdfLaTeX, dos pasadas):**

```
pdflatex presentacion.tex
pdflatex presentacion.tex
```

La segunda pasada es necesaria para que el contador `\inserttotalframenumber`
(el "/ 25" del pie) quede correcto.

**En Overleaf:** un proyecto de Overleaf tiene un único documento principal.
Como `main.tex` (la tesis) ya ocupa ese lugar, hay dos opciones:

1. Crear un **proyecto aparte** para la presentación y subirle `presentacion.tex`
   más la carpeta `imagenes/` (ajustando las rutas `../imagenes/` a `imagenes/`).
2. Cambiar temporalmente el documento principal desde
   *Menu > Settings > Main document*.

## Estructura (25 diapositivas, ~20 minutos)

| # | Contenido | Expositor |
|---|-----------|-----------|
| 1–2 | Portada y contenido | — |
| 3–11 | Problema, objetivos, solución, stack, arquitectura y 3 decisiones de diseño | Matías Melgarejo |
| 12–24 | Módulos, recorrido por la aplicación, cambios, validación y conclusiones | Matías Gaona |
| 25 | Cierre / preguntas | — |

El reparto está escrito en las dos diapositivas de sección (`\seccion{}{}`);
para cambiarlo alcanza con editar esos dos nombres.

## Ritmo sugerido

Las diapositivas de sección y las de captura de pantalla pasan rápido (10–20 s).
El grueso del tiempo está en arquitectura, decisiones de diseño, cambios respecto
al anteproyecto y objetivos alcanzados.
