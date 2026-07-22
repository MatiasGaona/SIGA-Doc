# Guía de estilo — conversión Markdown → LaTeX (SIGA Documentación Técnica)

No forma parte del documento compilado — es la referencia que sigue todo capítulo/apéndice para que el estilo sea uniforme en todo el proyecto. Leer `preamble.tex` también: ahí están todos los paquetes, colores y estilos TikZ ya definidos. **No agregar `\usepackage` nuevos en un capítulo** — si falta algo, anotarlo en el reporte final en vez de agregarlo.

**Regla de tono (2026-07-10):** esta documentación es para una tesis, no un registro de auditoría. Nunca usar los entornos `cajaAdvertencia`/`cajaRiesgo`/`\Atencion` para bugs, deuda técnica, referencias a `CLAUDE.md`, fechas de "verificado el...", hashes de commit, o notas de "esto lo corregí en la documentación". Si aparece algo de eso al escribir o actualizar un capítulo, anotarlo en `HALLAZGOS-PENDIENTES.md` (en la raíz del proyecto, tampoco forma parte del PDF) y dejar en el `.tex` solo la afirmación directa de cómo funciona el sistema hoy — estilo "Decisión." (ver ejemplos ya escritos en `capitulos/02-modelo-de-datos.tex`), sin hedging.

## Mapeo Markdown → LaTeX

| Markdown | LaTeX |
|---|---|
| `# Título` (nivel de módulo/ADR) | `\chapter{Título}` (+ `\label{mod:08-ventas}` o `\label{adr:0007}`) |
| `## Sección` | `\section{...}` (+ `\label{sec:...}`) |
| `### Subsección` | `\subsection{...}` |
| `**negrita**` | `\textbf{...}` |
| `` `código` `` | `\code{código}` — **nunca** escapar `_`/`%`/`&` a mano adentro, `\code` ya lo hace |
| bloque ```` ```csharp ... ``` ```` | `\begin{lstlisting}[language=csharpish]` ... `\end{lstlisting}` (o sin `language=` si es genérico) |
| tabla Markdown | `longtable` con `\toprule`/`\midrule`/`\bottomrule` de `booktabs` (nunca `\hline` suelto) |
| link `[texto](./otro.md)` | `\hyperref[sec:x]{texto}` si es interno (mismo documento), o texto plano + nota si era a un doc fuera de alcance (ej. memoria de proyecto, manuales) |
| ✅ Implementado | `\Implementado` |
| 🟡 Planificado | `\Planificado` |
| ⚠️ nota corta inline | `\Atencion\ texto...` |
| ⚠️ nota larga / bloque de riesgo | `\begin{cajaAdvertencia}texto\end{cajaAdvertencia}` (título opcional con `[Título]`, ej. `\begin{cajaAdvertencia}[Hallazgo de código]`) o `\begin{cajaRiesgo}` si es un riesgo serio (ej. aislamiento multi-sucursal manual, mismo patrón de título opcional) |
| diagrama Mermaid `erDiagram` | figura TikZ con estilo `entidad` (ver abajo) |
| diagrama Mermaid `graph`/`graph TB`/`graph LR` | figura TikZ con estilo `capa`/`subcapa`/`externo`/`flechaArq` |
| diagrama Mermaid `sequenceDiagram` | figura TikZ con estilo `actor`/`lineavida`/`mensaje` |

## Texto plano fuera de `\code{}`

Cualquier `_ % & # $ ~ ^` que aparezca en prosa normal (no dentro de `\code{}` ni de un `lstlisting`) tiene que escaparse a mano: `\_ \% \& \# \$ \textasciitilde{} \textasciicircum{}`. La regla práctica: **todo identificador de código** (nombre de tabla, permiso, ruta de endpoint, nombre de clase/propiedad) va envuelto en `\code{...}` — así nunca hay que pensar en el escape.

## Diagramas TikZ — patrones de referencia

**Arquitectura/componentes** (nodos = capas o sistemas externos):
```latex
\begin{figure}[htbp]
\centering
\begin{tikzpicture}[node distance=1.2cm]
  \node[capa] (api) {SIGA.Api};
  \node[capa, below=of api] (app) {SIGA.Application};
  \draw[flechaArq] (api) -- (app);
\end{tikzpicture}
\caption{...}
\label{fig:arq-...}
\end{figure}
```

**Entidad-relación** (nodo `entidad` es `rectangle split parts=2`: primer compartimento = nombre en negrita azul, segundo = lista de campos separados por `\\`):
```latex
\node[entidad] (persona) {\textbf{\textcolor{sigaAzul}{Person}} \nodepart{two} Id (PK) \\ CI (UK) \\ FirstName \\ LastName};
\node[entidad, right=2.5cm of persona] (user) {\textbf{\textcolor{sigaAzul}{User}} \nodepart{two} Id (PK) \\ PersonId (FK)};
\draw[relacion] (persona) -- node[above,font=\tiny]{1:1} (user);
```

**Secuencia** (actores en fila arriba, líneas de vida hacia abajo, mensajes horizontales entre líneas de vida a distinta altura — no hay librería de "sequence diagram" automática, se arma a mano con nodos + `\draw[mensaje]` entre coordenadas a distinto `y`):
```latex
\node[actor] (a) {Cliente};
\node[actor, right=3cm of a] (b) {VentaService};
\draw[lineavida] (a.south) -- ++(0,-6);
\draw[lineavida] (b.south) -- ++(0,-6);
\draw[mensaje] ($(a.south)+(0,-1)$) -- node[above,font=\scriptsize]{CrearVenta} ($(b.south)+(0,-1)$);
```

Si un diagrama de secuencia tiene muchos pasos (más de ~6-7), está bien partirlo en 2 figuras consecutivas en vez de una sola apretada.

## Convención de `\label`

- `sec:<capitulo>-<slug>` — ej. `sec:arch-multisucursal`
- `fig:<tipo>-<slug>` — ej. `fig:er-identidad`, `fig:seq-venta-a-pedido`
- `tab:<slug>` — ej. `tab:api-auth`, `tab:enums`
- `mod:<numero>-<slug>` — ej. `mod:08-ventas` (uno por capítulo de módulo)
- `adr:<numero>` — ej. `adr:0007`

No repetir un `\label` entre archivos — si dos capítulos necesitan referirse a la misma idea, cada uno tiene su propio label y uno referencia al otro con `\hyperref[label-del-otro]{texto}`.

**`cleveref` NO está cargado en el preámbulo** — no usar `\cref{}`/`\Cref{}`. Usar `Figura~\ref{fig:...}`, `Capítulo~\ref{mod:...}`, `\S\ref{sec:...}` a mano (con `~` para el espacio no separable antes del número). `hyperref` ya hace que `\ref` sea clickeable, no hace falta nada más.

Los capítulos de fundaciones (Arquitectura, Modelo de Datos, Arquitectura del Frontend) usan `\label{sec:arch}`, `\label{sec:modelo-datos}` y `\label{sec:frontend}` como label raíz del capítulo — usar esos, no inventar otros, si hay que referenciarlos desde un módulo (ej. `Capítulo~\ref{sec:arch}, \S\ref{sec:arch-multisucursal}`).

Si necesitás citar un `\label` que otro fork todavía no escribió (ADR, u otro módulo que se está escribiendo en paralelo ahora mismo), usá `\hyperref[label-esperado]{texto}` siguiendo la convención (`adr:0007`, `mod:09-laboratorio`, etc.) — va a dar "reference undefined" hasta que se compile con todo el documento junto al final, eso es esperado.

## Cuidado: `\code{}` NUNCA lleva backslash adentro (regla encontrada y corregida 2026-07-10)

`\code{}` usa `\detokenize`, que convierte **cualquier comando** (`\{`, `\}`, `\&`, `\_`, `\%`...) a su representación literal de dos caracteres (backslash + símbolo) en vez de al símbolo solo. Esto se encontró roto en ~170 lugares (rutas con parámetros y query strings) y ya se corrigió en todo el proyecto — la regla para lo que se escriba de acá en más:

- **Dentro de `\code{...}`: nunca escapar nada.** `_`, `&`, `#`, `$`, y llaves balanceadas `{...}` van TAL CUAL, sin backslash — `\detokenize` los reproduce correctos solos.
  - Ruta con parámetro: `\code{/api/users/{id}/reset-password}` (llaves SIN escapar).
  - Identificador con guión bajo: `\code{ver_pacientes}`, `\code{vw_stock_actual}` (SIN escapar).
  - Condición C# con `&&`: `\code{a == "x" && b == "y"}` (SIN escapar) — **excepto** si esa `\code{}` está dentro de una celda de `longtable`/`tabular`: ahí un `&` de verdad corta la columna aunque esté "dentro" de `\code{}`, así que en celdas de tabla usar `\texttt{...\&...}` (con `\&` escapado a mano, y `\texttt` en vez de `\code`) para ese caso puntual.
- **`%` es la ÚNICA excepción a lo anterior: nunca puede ir dentro de `\code{}`** (regla corregida 2026-07-16 — la versión previa de esta guía lo listaba como seguro, y eso rompía el documento). Motivo: `%` tiene catcode 14 (comentario) y TeX lo descarta **en el lexer**, antes de que `\detokenize` llegue a verlo — a diferencia de `_ & # $`, que sí se tokenizan y por eso `\detokenize` los recupera. Con `\code{%q%}`, TeX se come el resto de la línea *incluida la llave de cierre* y sigue buscándola en las líneas siguientes: `Runaway argument` / llave sin balancear. Para mostrar un `%` (patrones `LIKE '%q%'`, porcentajes en `\texttt`) usar `\texttt{...\%...}` con `\%` escapado a mano, igual que en el caso del `&` en celda de tabla.
- **Si necesitás mostrar llaves literales SUELTAS** (no una ruta, ej. un shape `{ items, totalCount }`), usá `\texttt{\{ items, totalCount \}}` (con `\{`/`\}` escapados) — ahí SÍ hay que escapar porque no hay `\detokenize` de por medio.

Resumen: `\code{}` = auto-escapa todo, nunca poner backslash adentro. Dos excepciones, y en ambas se usa `\texttt{}` con escape manual en vez de `\code{}`: un `&` dentro de una celda de tabla, y un `%` en cualquier lado. `\texttt{}` directo = escapar todo a mano como en cualquier LaTeX normal.

## Cuidado: `\code{}` NUNCA dentro de un argumento móvil (encontrado y corregido 2026-07-10, vía log de Overleaf)

`\chapter{...}`, `\section{...}`, `\subsection{...}` y `\caption{...}` son "argumentos móviles": su contenido se escribe también al `.toc`/`.lof`/marcadores PDF de hyperref, y se re-procesa ahí. `\detokenize` (la base de `\code{}`) no sobrevive ese segundo paso — provoca `! Missing $ inserted.` / `! Extra }, or forgotten $.` / `Runaway argument?` en cascada, uno por cada capítulo que tenga un título con `\code{}` adentro. Esto rompió ~230 subsecciones del Apéndice de API y varias `\caption` del capítulo de Modelo de Datos.

**Regla:** dentro de `\chapter{}`, `\section{}`, `\subsection{}`, `\caption{}` (y el primer argumento de `apiTable`, que termina en un `\caption{#1}`) — **nunca usar `\code{}`, usar `\texttt{}` con escape manual**, igual que en celdas de tabla:
- `\subsection{\texttt{UserRolesController} --- \texttt{/api/users/\{userId\}/roles}}` (braces escapadas a mano).
- `\caption{... \texttt{HasConversion<int>()} ...}` (`<`/`>` no necesitan escape en `\texttt`, ver regla siguiente).

Fuera de esos 4 comandos (prosa normal, celdas de tabla que no sean `\caption`, `\hyperref[]{}`), `\code{}` sigue siendo la opción correcta — la regla de arriba sobre `\code{}` en texto plano no cambia.

## Cuidado: `<` y `>` sueltos (encontrado y corregido 2026-07-10)

El módulo `spanish` de babel activa `<` y `>` como shorthand de `<<...>>` (comillas angulares `« »`). Esto rompe cualquier `<`/`>` suelto — muy común acá por genéricos de C# (`Result<T>`, `List<T>`, `HasConversion<int>()`) — con errores de `\language@active@arg`. El preámbulo ya tiene `\shorthandoff{<>}` (este documento no usa comillas angulares en ningún lado), así que **`<`/`>` se escriben tal cual, sin escapar, en cualquier lado** (prosa, `\texttt{}`, `\code{}`, títulos). No agregar `\ProvideTextCommand`/`\textless`/`\textgreater` a mano, no hace falta.

## Cuidado: `longtable` nunca usa columnas `X`/`Y` (encontrado y corregido 2026-07-10)

El preámbulo define `\newcolumntype{Y}{>{\raggedright\arraybackslash}X}` (de `tabularx`) pero **`tabularx` y `longtable` son incompatibles** — una columna `X`/`Y` dentro de `\begin{longtable}{...}` tira `! Package array Error: >{..} at wrong position` seguido de `! Missing # inserted in alignment preamble` en cascada en cada `\endfirsthead`/`\endhead`/`\endfoot`. **Todas las tablas de este documento son `longtable`** (nunca `tabularx` puro) — usar siempre `p{Ncm}` con ancho fijo para cada columna, nunca `X`/`Y`. El `\newcolumntype{Y}` queda definido en el preámbulo por si algún día se necesita un `tabularx` real (tabla corta, sin salto de página), pero hoy no se usa en ningún lado.

## Bloques `lstlisting` (código, árboles de carpetas)

- Tildes/ñ (`á é í ó ú ñ ü ¿ ¡`) andan bien dentro de `lstlisting`/`\lstinline` — el preámbulo ya tiene la tabla `literate` que las traduce. No hace falta evitarlas.
- Los caracteres de dibujo de caja Unicode (`├ └ │ ─ ┌`) **no** están cubiertos y no hay que usarlos — para árboles de carpetas usar ASCII: `|--` y `` `-- ``.

## Qué NO hacer

- No usar emoji reales (✅⚠️🟡) en el `.tex` — siempre los comandos (`\Implementado`, etc.) o las cajas.
- No usar `\hline` en tablas — siempre `booktabs` (`\toprule`/`\midrule`/`\bottomrule`).
- No agregar `\usepackage` nuevos — si hace falta algo que no está en `preamble.tex`, dejarlo anotado en el reporte final en vez de resolverlo con un paquete nuevo.
- No dejar un diagrama Mermaid sin convertir "para después" — todos se convierten en esta pasada (son ~25 en total, ver el plan).
