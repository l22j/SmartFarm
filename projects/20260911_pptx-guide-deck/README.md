# pptx-guide-deck

- Canvas format: ppt169
- Created: 20260911

## Directories

- `svg_output/`: raw SVG output
- `svg_final/`: self-contained SVG visual preview; may be inserted manually as an SVG image, but PowerPoint Convert to Shape is unsupported
- `images/`: runtime image pool; converter assets keep their original short filenames when possible
- `icons/`: project icon set — selected library icons copied in (via icon_sync.py) plus any custom icons you add; embedded from here at export
- `notes/`: speaker notes (created on demand — only when notes are requested)
- `templates/`: project templates (created on demand — only when a template is installed)
- `live_preview/`: browser preview runtime files and history (lock.json, server.log, edits.jsonl, annotations.jsonl)
- `sources/`: source materials and normalized markdown
- `analysis/`: machine-extracted intermediate analysis (PPTX intake, image_analysis.csv) — the pipeline's canonical must-read source/asset facts
- `exports/`: native DrawingML pptx (`<title>_ver1.pptx`, `_ver2`, ... auto-increment); `_native_charts` suffix with `--native-objects`, `_narrated` suffix when narration audio is embedded
- `backup/<timestamp>/`: svg_output/ archive (always written in default-flow mode; safe to delete old timestamps)
