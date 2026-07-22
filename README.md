# SIGA-Óptica — Documentación Técnica (Trabajo Final de Grado)

Informe del trabajo final de grado sobre **SIGA-Óptica**, un sistema de gestión
integral para una óptica: pacientes, historia clínica, agenda, inventario,
ventas, laboratorio, compras, caja y operación multi-sucursal.

Autores: Matías Gaona y Matías Melgarejo.

El sistema documentado vive en otros dos repositorios:

| Repositorio | Contenido |
|-------------|-----------|
| `SIGA` | Backend — ASP.NET Core (.NET 10), EF Core, PostgreSQL |
| `SIGA-Web` | Frontend — Vue 3 + TypeScript + Vite |

## Contenido

- `main.tex` — documento principal; encadena todos los `\include`.
- `preamble.tex` — paquetes, paleta institucional, estilos TikZ y comandos propios.
- `portada.tex` — carátula.
- `capitulos/` — introducción, resumen, arquitectura, modelo de datos, los 16
  módulos de negocio, implementación, validación, cambios respecto al
  anteproyecto y conclusiones.
- `apendices/` — referencia de la API (235 endpoints), 13 decisiones de diseño
  (ADR), manual de usuario y manual técnico.
- `imagenes/` — capturas de pantalla de la aplicación, una por módulo.
- `presentacion/` — presentación Beamer para la defensa (ver su propio `LEEME.md`).
- `auxiliares/` — material de referencia: Plan de Desarrollo y plantilla del TFG.

## Cómo compilar

Con una distribución de LaTeX local (pdfLaTeX + Biber):

```sh
pdflatex main.tex
biber main
pdflatex main.tex
pdflatex main.tex
```

Las pasadas repetidas son necesarias para que cuadren el índice, las
referencias cruzadas y la numeración de figuras. Resultado actual: 186 páginas,
sin errores ni referencias indefinidas.

Para compilar en Overleaf, ver [`LEEME-OVERLEAF.md`](LEEME-OVERLEAF.md).

## Antes de escribir

- [`GUIA-ESTILO.md`](GUIA-ESTILO.md) — criterios de redacción del documento.
- [`guia_latex_tfg.md`](guia_latex_tfg.md) — convenciones de LaTeX del proyecto.
- [`HALLAZGOS-PENDIENTES.md`](HALLAZGOS-PENDIENTES.md) — deuda detectada al
  contrastar la documentación contra el código, y lo que queda por resolver.

> La fuente de verdad de este documento es el código. Las cifras (cantidad de
> endpoints, entidades, migraciones, vistas) **derivan rápido**: conviene
> recontarlas contra el repositorio antes de cada entrega, en vez de confiar en
> lo que ya está escrito.
