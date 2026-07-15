# TECH-CUP-2026-Docs

Repositorio de documentación centralizada de la plataforma **TECH CUP 2026** ([TECH-CUP-2026-INT](https://github.com/TECH-CUP-2026-INT)), construido con [MkDocs](https://www.mkdocs.org/) y el tema [Material for MkDocs](https://squidfunk.github.io/mkdocs-material/).

## Contenido

- Inicio (índice de repositorios de la organización)
- Análisis de Requerimientos
- Documento de Arquitectura
- Pruebas Funcionales
- Organización

## Desarrollo local

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
mkdocs serve
```

El sitio quedará disponible en `http://127.0.0.1:8000`.

## Publicación

El sitio se publica automáticamente en GitHub Pages mediante el workflow `.github/workflows/deploy.yml` en cada push a `main`. Para habilitarlo, activa GitHub Pages en la configuración del repositorio con la fuente **GitHub Actions**.
