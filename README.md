# Descriptive statistics for the supervisor (meeting of 22 October 2026)

- `descriptive_statistics_road_financing.html`: the page (English, animated, Chart.js from cdnjs; one file, open in a browser; needs internet for the chart library and the fonts).
- `artifact_page.html`: the same page in the format of the Claude Artifact (no html/head/body wrapper); published as a private artifact.
- `capa/`: Arthur's cover (Home.pdf) and the photo extracted from it (cover_photo.jpg), embedded in the page.
- `metadata_descriptive_statistics.xlsx`: one sheet per chart or table with its data, plus `Index` (id, title, source, period, unit, coverage) and `Tests`.
- `presentation_script.md`: the script, in English, with the numbers read from the same tables.
- `dados/`: the 67 CSVs (one per chart or table), `index.csv`, `testes.csv`, `spot_checks.csv`.

Build, from the root `padrao_canais`:
```
python3 codigo/44_descritiva_orientadora_dados.py   # tables, xlsx, tests and spot checks
python3 codigo/45_descritiva_orientadora_html.py    # page, artifact page, script, README
```
Inputs (read only): `relatorio/descritiva_sp_mg/dados/*.csv` (scripts 14 to 42), `dados_filtrados/<UF>/ano_canal.csv`, `../credito_externo/dados_filtrados/{final,macro}`, `~/Documents/tese_dashboard/dash/base.json`, `~/Documents/tese_dashboard/tracing_note/...` (project base, credit universe, state accounts 2001-2012).
Colours: fixed per channel (1a federal, 1a state Treasury, 1a state fund, 1b, 1b CIDE, 2, 3, 3/4, 4, 5, no channel) and per level of government; every adjacent pair validated with the dataviz palette checker (lightness, chroma, colour-deficient and normal-vision separation).
Tests on the last build: 95/95 (29/29 spot checks on random contracts).
