# Cómo subir esto a Overleaf

## 1. Subir el proyecto

En [overleaf.com](https://www.overleaf.com), botón **New Project → Upload Project**, y subí esta carpeta completa como `.zip` (todo el contenido de `siga-documentacion-tecnica/`, no la carpeta contenedora). Alternativa si tenés Overleaf Premium con integración Git: podés crear un repo git local de esta carpeta y usar la sincronización Git de Overleaf en vez de subir un zip.

## 2. Motor de compilación

Overleaf detecta el motor automáticamente en la mayoría de los casos, pero si compila raro: **Menu (arriba a la izquierda) → Compiler → pdfLaTeX**. Todo el documento está pensado para `pdflatex` (no hace falta XeLaTeX/LuaLaTeX).

## 3. Completar la carátula

Antes de la primera entrega, abrí `portada.tex` y completá los campos `[Completar: ...]`: nombre de la universidad, facultad/carrera, tu nombre, tutor/a, ciudad y año. Si tu universidad exige un formato de carátula específico, reemplazá ese archivo entero por la plantilla oficial — el resto del documento no depende de su contenido.

## 4. Primera compilación

Puede tardar 30-60 segundos la primera vez (documento grande, ~25 diagramas TikZ). Si Overleaf tira un error de compilación:

- **Copiá el mensaje de error completo** (con el número de línea y el archivo que indica) y pasámelo — no hace falta que lo diagnostiques vos, yo lo reviso y corrijo el archivo puntual.
- Este documento se armó y revisó **sin poder compilar localmente** (no había motor LaTeX instalado en la máquina donde se escribió) — se hizo una revisión manual exhaustiva de sintaxis (balance de `\begin`/`\end`, de llaves, de `\label`/`\ref`, caracteres especiales), pero la primera compilación real en Overleaf es la primera verificación de punta a punta. Es esperable necesitar 1-2 rondas de ajustes menores.

## 5. Compilar solo un capítulo mientras se corrige algo

Si un solo capítulo da problemas y querés iterar rápido sin recompilar las ~2800 líneas del documento completo, en `main.tex` descomentá la línea:

```latex
\includeonly{capitulos/02-modelo-de-datos}
```

(cambiando la ruta al capítulo que estés tocando) y comentala de nuevo cuando termines, para volver a compilar todo.

## 6. Estructura del documento

- `main.tex` — documento principal, todos los `\include`.
- `preamble.tex` — paquetes, colores, estilos de TikZ compartidos, comandos (`\Implementado`, `\Atencion`, `\code{}`, cajas `cajaAdvertencia`/`cajaRiesgo`/`cajaADR`). **No agregar paquetes nuevos acá sin necesidad real** — todo el documento fue escrito para funcionar solo con lo que ya está.
- `portada.tex` — carátula (completar campos, ver punto 3).
- `capitulos/01` a `03` — Arquitectura, Modelo de Datos, Arquitectura del Frontend.
- `capitulos/04-01` a `04-15` — los 15 módulos de negocio.
- `apendices/a-referencia-api.tex` — los 230 endpoints de la API.
- `apendices/b-decisiones-diseno.tex` — las 12 decisiones de diseño (ADR).
- `GUIA-ESTILO.md` — no forma parte del PDF, es la referencia de convenciones si en algún momento agregás o editás contenido vos mismo (o pedís que yo lo haga). Vale la pena leerla antes de tocar el `.tex` a mano, tiene varios gotchas de LaTeX ya resueltos (especialmente la sección sobre `\code{}`).

## 7. Alcance de este documento

Es **solo la documentación técnica** (arquitectura, modelo de datos, módulos de negocio, referencia de API, decisiones de diseño) — no incluye Introducción, Objetivos, Marco Teórico, Metodología ni Conclusiones del trabajo final de grado; esas secciones quedan para vos, y podés agregarlas como capítulos nuevos antes/después de las `\part{}` existentes sin tocar lo que ya hay.

## 8. Si querés actualizar contenido más adelante

El código fuente real (`SIGA`/`SIGA-Web`) y la documentación en Markdown (`SIGA/docs/`, `SIGA-Web/docs/`) siguen siendo la fuente de verdad. Si el sistema cambia después de esta conversión, lo más simple es pedirme que actualice el `.md` correspondiente primero y después migre el cambio al `.tex` — así ambas versiones (Markdown y LaTeX) quedan sincronizadas.
