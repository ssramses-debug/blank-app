# AGENTS.md

## Project overview

This repository hosts a Streamlit app. The current business goal is to build and maintain an interactive Spanish-language dashboard equivalent to the provided "Dashboard de Cobertura Nacional" specification.

## Source-of-truth product requirements

When implementing or updating the dashboard, preserve these behaviors:

1. **Header and filter**
   - Title: `Panel Interactivo de Estado de Fuerza`.
   - Region selector with: `consolidado`, `leon_ags`, `cdmx`, `bajio`.
   - Region change pre-fills all editable inputs and table values.

2. **Editable inputs (direct variables)**
   - `Tiempo Proyectado` (`tp`)
   - `Tiempo Cubierto` (`tc`)
   - `Vacaciones e Incapacidades` (`vac`)
   - `Faltas e Incidencias` (`faltas`)

3. **Coverage KPI card**
   - Base formula: `(tc / tp) * 100`.
   - Display with one decimal place and `%`.
   - Color thresholds:
     - `>= 95`: blue (`#1565C0`)
     - `90-94.9`: green (`#4CAF50`)
     - `80-89.9`: yellow (`#FFC107`)
     - `< 80`: red (`#F44336`)
   - Compatibility rule from the provided spec: if `tp=120` and `tc=105`, display exactly `95.0%`.

4. **Incidence charts**
   - Pie chart uses raw values: `[vac, faltas]`.
   - Horizontal stacked bar chart normalizes to 100%:
     - `pctVac = vac / (vac + faltas) * 100`
     - `pctFaltas = faltas / (vac + faltas) * 100`
   - Handle zero-total incidence safely (both percentages should be `0`).

5. **Consolidated force table**
   - Inputs:
     - Radio: `radioProy`, `radioReal`
     - Supervisores: `supProy`, `supReal`
   - Derived fields:
     - `Suma Total = radioProy + radioReal + supProy + supReal`
     - `Promedio = (radioReal + supReal) de (radioProy + supProy)`
   - Region label must reflect selected region name.

6. **Region preset data**
   - Keep/update this baseline map:
     - `consolidado`: `tp=120, tc=105, vac=11, faltas=4, rp=2, rr=2, sp=3, sr=1, name="Leon-Ags"`
     - `leon_ags`: `tp=100, tc=88, vac=2, faltas=5, rp=2, rr=2, sp=3, sr=1, name="Leon-Ags"`
     - `cdmx`: `tp=150, tc=140, vac=0, faltas=10, rp=2, rr=2, sp=2, sr=2, name="CDMX"`
     - `bajio`: `tp=80, tc=70, vac=1, faltas=3, rp=1, rr=1, sp=2, sr=1, name="Bajío"`

## Implementation guidelines

- Keep all user-facing labels in Spanish unless explicitly asked to translate.
- Prefer readable variable names matching the domain (`tp`, `tc`, `vac`, `faltas`, etc.).
- Guard against divide-by-zero in every percentage calculation.
- If implementing with JavaScript chart libraries, always update nested dataset arrays with explicit indices.
- If implementing directly in Streamlit, maintain equivalent visual semantics even if underlying chart libraries differ.

## Testing guidance (must run for non-trivial code changes)

1. Install dependencies:
   - `pip install -r requirements.txt`
2. Run app locally:
   - `streamlit run streamlit_app.py`
3. Manual verification checklist:
   - Switch each region and confirm all fields update correctly.
   - Edit each numeric input and verify KPI card text/color changes immediately.
   - Confirm pie and 100% bar charts reflect the edited incidence values.
   - Confirm table `Suma Total` and `Promedio` recalculate correctly.
   - Verify zero-incidence case (`vac=0`, `faltas=0`) does not crash and renders 0%.

## Out of scope unless requested

- Do not add authentication, persistence, or backend APIs.
- Do not replace the dashboard interaction model with multi-page navigation.
- Do not change KPI thresholds or region baseline values without explicit direction.
