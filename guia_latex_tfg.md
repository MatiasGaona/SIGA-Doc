# Guía de estructura LaTeX — TFG Proyecto II (SIGA-Óptica)

> Basado en: `Plantilla_TFG_Proyecto_II_v1_-_comentada.docx` (Facultad Politécnica – UNA)
>
> Propósito: traducir la plantilla institucional (Word) a una especificación de trabajo para redactar el documento final en LaTeX/Overleaf, manteniendo la estructura y el contenido exigidos por la facultad.

---

## 1. Especificaciones de formato detectadas

La plantilla Word usa estos valores (no parecen ser una tipografía institucional deliberada, sino los valores por defecto de Word — **confirmar con secretaría académica si exigen fuente/tamaño específicos**; si no hay exigencia explícita, estos son un punto de partida razonable):

| Propiedad | Valor detectado | Equivalente LaTeX |
|---|---|---|
| Tamaño de página | Carta (8.5" × 11") | `letterpaper` |
| Márgenes sup./inf. | 1" (2.54 cm) | `top=2.54cm, bottom=2.54cm` |
| Márgenes izq./der. | 1.25" (3.17 cm) | `left=3.17cm, right=3.17cm` |
| Fuente cuerpo | Calibri 11 pt | Sin exigencia clara → usar Latin Modern / Times New Roman 11-12pt |
| Fuente títulos | Cambria (negrita, con color de acento) | En LaTeX, títulos en negro/negrita estándar (evitar color salvo que se pida) |
| Interlineado | ~1.15 | `\usepackage{setspace}` + `\onehalfspacing` o `\singlespacing` según lo que pida la facultad |

Preámbulo sugerido de partida:

```latex
\documentclass[12pt,letterpaper]{report}
\usepackage[utf8]{inputenc}
\usepackage[spanish]{babel}
\usepackage[left=3.17cm,right=3.17cm,top=2.54cm,bottom=2.54cm]{geometry}
\usepackage{setspace}
\usepackage{graphicx}
\usepackage{caption}
\usepackage{booktabs}
\usepackage{hyperref}
\usepackage[backend=biber,style=apa]{biblatex}
```

**Nota:** la plantilla no define un esquema de numeración automática de títulos (los números "1.", "2.", etc. están escritos a mano en el Word, no generados por Word). En LaTeX esto se resuelve automáticamente con `\chapter{}` — no hace falta escribir el número.

---

## 2. Estructura del documento

Usar clase `report` → cada sección numerada del Word es un `\chapter`. Portada, índice, bibliografía y anexos van fuera del conteo de capítulos.

### Organización de archivos sugerida (Overleaf)

```
main.tex
portada.tex
capitulos/
  01_introduccion.tex
  02_resumen_proyecto.tex
  03_arquitectura.tex
  04_construccion_software.tex
  05_base_datos.tex
  06_implementacion.tex
  07_validacion.tex
  08_cambios_proyecto1.tex
  09_conclusiones.tex
bibliografia.bib
anexos/
  anexo_manual_usuario.tex
  anexo_manual_tecnico.tex
  anexo_scripts.tex
```

`main.tex` los incluye con `\include{}` o `\input{}` en orden.

---

### Portada

**Contenido obligatorio:** Universidad, Facultad, Carrera, Título del proyecto, Autores, Tutor, Año.

⚠️ El título **debe coincidir exactamente** con el aprobado en Proyecto I ("SIGA-Óptica" + subtítulo formal, verificar redacción exacta usada en la aprobación).

Implementar como página de título manual (`\begin{titlepage}...\end{titlepage}`) en vez del comando `\maketitle` genérico, para controlar el layout institucional.

---

### Índice

`\tableofcontents` — se genera solo, no requiere mantenimiento manual (a diferencia de Word).

---

### Capítulo 1 — Introducción

No repetir el análisis completo de Proyecto I; solo referenciarlo brevemente.

Contenido a cubrir:
- Antecedentes (mención breve de Proyecto I / el problema del Centro Óptico Santa María)
- Objetivo del documento (qué reporta este entregable de Proyecto II)
- Alcance (qué quedó dentro y fuera del sistema implementado)

### Capítulo 2 — Resumen del Proyecto

Contenido a cubrir:
- Problema que resuelve SIGA-Óptica
- Solución propuesta (resumen ejecutivo del ERP)
- Alcance definitivo (módulos entregados: inventario, ventas, compras, egresos, catálogo, notificaciones, etc.)

### Capítulo 3 — Arquitectura de la Solución

Contenido a cubrir:
- Arquitectura (Vue 3 + Vite + Vuetify / API REST .NET / PostgreSQL 16 / Docker + Caddy) — incluir diagrama general (componentes, despliegue)
- Tecnologías utilizadas y versión
- Justificación de las decisiones (por qué esta arquitectura y no otra)

Diagramas: usar TikZ, o exportar de una herramienta externa (draw.io, Mermaid) como PDF/PNG e incluir con `\includegraphics`.

### Capítulo 4 — Construcción del Software

Uno por módulo implementado (Inventario/Stock, Ventas, Compras, Egresos, Catálogo, Notificaciones, Clínico, etc.). Evitar copiar código fuente completo — solo fragmentos explicativos puntuales si aportan claridad.

Por cada módulo:
- Objetivo del módulo
- Funcionalidades principales
- Reglas de negocio clave (ej.: descuento de stock en `comprobante.emitido`, modelo plano de producto, etc.)
- Capturas de pantalla (con numeración y explicación — ver sección 4 de recomendaciones)
- Observaciones / decisiones de diseño relevantes

### Capítulo 5 — Base de Datos

Documentar solo tablas y objetos **principales** (no todo el esquema exhaustivamente).

Contenido a cubrir:
- Modelo físico (diagrama entidad-relación)
- Diccionario de datos (tablas clave: `producto`, `movimiento_inventario`, `stock_sucursal`, `comprobante`, etc.)
- Funciones y triggers relevantes

### Capítulo 6 — Implementación

Contenido a cubrir:
- Requisitos (hardware/software del servidor — VPS, Docker, etc.)
- Instalación (pasos de despliegue, `docker-compose.prod.yml`)
- Configuración (variables de entorno, Caddy, dominio, SSL)

### Capítulo 7 — Validación

Contenido a cubrir:
- Casos de prueba ejecutados
- Resultados obtenidos
- Capturas de evidencia

### Capítulo 8 — Cambios respecto al Proyecto I

Contenido a cubrir:
- Diseño inicial (qué se planteó en Proyecto I)
- Problema encontrado durante el desarrollo
- Solución adoptada
- Justificación del cambio

(Ejemplo real del proyecto: corrección del trigger de descuento de stock, que pasó de "al entregar" a "al emitir comprobante" — este tipo de decisión documentada va acá.)

### Capítulo 9 — Conclusiones

Contenido a cubrir:
- Objetivos alcanzados
- Limitaciones
- Mejoras futuras

---

### Bibliografía

Formato **APA**, solo fuentes efectivamente consultadas y citadas en el texto.

```latex
\usepackage[backend=biber,style=apa]{biblatex}
\addbibresource{bibliografia.bib}
...
\printbibliography
```

### Anexos

Contenido sugerido:
- Manual de usuario
- Manual técnico
- Scripts (SQL, migraciones)
- Código QR al repositorio
- Otra documentación complementaria

Usar `\appendix` antes de este bloque para que la numeración cambie a A, B, C...

---

## 3. Checklist de redacción (recomendaciones generales de la plantilla)

Estas NO son un capítulo — son criterios de calidad a aplicar en todo el documento:

- [ ] Lenguaje técnico y objetivo en toda la redacción
- [ ] No copiar código fuente salvo fragmentos pequeños y explicativos
- [ ] Todas las figuras y tablas numeradas (`\caption` + `\label` + `\ref` — automático en LaTeX, a diferencia de Word)
- [ ] Cada captura de pantalla acompañada de una explicación en el texto
- [ ] Revisión de ortografía, coherencia y formato antes de la entrega final
- [ ] Eliminar cualquier texto de guía/placeholder antes de la entrega (aplica al Word original; en LaTeX simplemente no se copian estas instrucciones al `main.tex` final)

---

## 4. Nota sobre esta guía

Este archivo es una guía de **estructura y contenido**, no el documento final. Sirve como referencia para:
1. Armar el esqueleto de archivos `.tex` en Overleaf
2. Verificar que ningún capítulo obligatorio quede afuera
3. Dar contexto consistente si se usa Claude Code (con acceso al repo) para redactar capítulos técnicos con datos reales del sistema (arquitectura, modelo de datos, etc.)

Pendiente de verificar con la facultad: si existe una plantilla LaTeX oficial ya armada, o si el formato exacto de tipografía/portada tiene requisitos más estrictos que los detectados en el Word (que parece usar el tema por defecto de Word, no un diseño institucional dedicado).
