# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Proyecto

Guia de antibioterapia en infecciones musculoesqueleticas para traumatologos. Herramienta clinica single-file HTML con dos archivos principales:

- **`index.html`** — App principal (~2100 lineas). Landing page con jerarquia: 3 cards primarias (infecciones) + 2 featured (antibiograma, liquido sinovial) + 3 secundarias (consulta). Secciones clinicas con resumen expandible, tratamiento empirico, secuencia terapeutica IV→Oral en cascada, datos ICM 2025, interpretacion de antibiograma y analizador de liquido sinovial.
- **`espectro-antibac.html`** — Tabla de espectro antibacteriano (42 antibioticos x 15 microorganismos) con filtros por familia, germen y actividad. Archivo independiente en nueva pestana.

## Sistema de diseno

Usa **Sistema B "Radiologia Glass"** del ecosistema COT (ver `/Users/jma/Documents/Proyectos/COT/CLAUDE.md`):
- Fuentes: Source Serif 4 (titulos) + Source Sans 3 (cuerpo) + Source Code Pro (mono)
- Header: Slate Steel `#1e293b → #334155 → #475569`, accent line `#38bdf8→#a78bfa`
- Glassmorphism: `backdrop-filter: blur()` + `background: rgba()` en cards/inputs
- Dark mode: `[data-theme="dark"]` con sky toggle CSS puro
- Iconos landing: emojis en `<span class="lc-emoji">` para cards (🦿🦴🩹🧫💉🛡️🦠🔬). SVG stroke-based inline (objeto `ICO`) para iconos internos de secciones
- Paleta: sky, violet, emerald, amber, red, slate (cada seccion tiene color acento)

## Arquitectura JS (index.html)

Los datos clinicos estan inline como arrays/objetos JS globales:
- `PROPHYLAXIS` — 11 procedimientos con dosis, timing, alternativas
- `ORGANISMS` — 18 microorganismos con tratamiento IV/oral, duraciones por escenario, alergias, y `dosisI` (dosis incrementada si sensibilidad intermedia)
- `OA_INFO` — 4 tipos de infeccion osteoarticular
- `PB_DATA` — 5 tipos de partes blandas
- `EMPIRIC` — Tratamiento empirico por escenario: `iap_dair`, `iap_rev`, `iap_mega`, `artritis`, `osteo_aguda`, `osteo_cronica`, `espondilo`
- `MUESTRAS_CHECKLIST` — Checklist toma de muestras (ICM 2025, Cap. 18)
- `FACTORES_RIESGO` — Factores de riesgo modificables para infeccion (ICM 2025, Cap. 1.4)
- `DOSIS_INCREMENTADAS` — Tabla EUCAST 2024: dosis estandar vs incrementada por familia de antibiotico
- `LS_TIPOS` — 6 tipos de liquido sinovial (normal, no inflamatorio, inflamatorio, septico nativo, protesica, hemartros) con puntos de corte y plan de actuacion
- `FAQ_DATA` — 9 preguntas frecuentes sobre antibiograma e interpretacion
- `SECTION_ANTIBIOGRAMA` / `SECTION_LIQUIDO` — Cards featured en landing (fila de 2, separadas de SECTIONS_SECONDARY)
- `SECTIONS_PRIMARY` / `SECTIONS_SECONDARY` — Cards de la landing con jerarquia
- `REF_URLS` — Mapa de URLs de referencias por patologia (PRIOAM, Macarena, IDSA, OVIVA, EUCAST)

Estado global: `allergies` (bl/qn/vc), `currentView`, sub-tabs (`currentIAPSub`, `currentOASub`, `currentPBSub`, `abgSub`), `microSelectedOrg`, `microScenario`, `profiCat`, `profiProc`, `_refContext`, `lsProtesica`.

Navegacion: `goLanding()` / `goSection(id)` muestran/ocultan divs. Micro section usa sidebar+content con DOM parcial (`selectMicroOrg`, `changeMicroScenario`).

## Seccion Antibiograma

5 sub-pestanas via `abgSub`:
- **sir** (`renderAbgSIR`): Cards S/I/R, comparativa Antes (slate) vs Ahora (sky) pre-2022 vs EUCAST 2024, quick reference SI/NO hacer
- **conceptos** (`renderAbgConceptos`): CMI (con error frecuente de comparar entre farmacos), PK/PD por clase con explicacion conceptual, penetracion osea con calculo, perfusion extendida (concepto + comparativa bolo vs 3h), escalada/desescalada
- **algoritmo** (`renderAbgAlgoritmo`): Arbol horizontal progresivo (3 niveles: S/I/R → protesica? → prescripcion+seguimiento como nodo unico). Niveles ocultos que se despliegan al clicar, toggle para deseleccionar. En movil: ramas descartadas se ocultan (display:none)
- **dosis** (`renderAbgDosis`): Tabla EUCAST con dosis estandar vs incrementada, link a eucast.org
- **faq** (`renderAbgFAQ`): 9 preguntas en accordion (incluye: SAMR, cuando llamar a Infecciosas, discordancia liquido/biopsia, cultivo negativo con sospecha)

## Seccion Liquido Sinovial

`renderLiquido()` con:
- Theory card ampliada: tecnica artrocentesis (NO anestesico intraarticular), muestras (min 2, ideal 3), hemocultivos ANTES del AB, tincion de Gram (sensibilidad 45%, NO descarta infeccion), puntos de corte nativa vs protesica
- Tabla de clasificacion: normal, no inflamatorio, inflamatorio, septico nativo, protesica (con puntos de corte diferenciados). Hemartros excluido de la tabla (solo en analizador)
- Nota destacada: protesis vs nativa (>3.000 leucos y >65% PMN ya sugiere infeccion)
- Analizador interactivo (`analizarLS()`): toggle nativa/protesica (`lsProtesica`), inputs leucocitos + %PMN + glucosa sinovial + glucemia simultanea + aspecto. Calcula ratio glucosa sinovial/sangre automaticamente. Clasifica y muestra plan de actuacion + recordatorio Gram

## Convenciones especificas

- Alergias modifican recomendaciones en cascada via `refreshCurrentView()`
- Tratamiento empirico (`renderEmpiric()`) visible en IAP y Osteoarticular antes del selector de microorganismo
- `dosisI` en ORGANISMS: bloque amber dentro de la card IV de la cascada con badge "I" y label "Si sensibilidad intermedia"
- Referencias clicables (`makeRef()`) con URLs especificas por patologia (contexto via `_refContext`)
- ICO object: iconos SVG con atributos inline `fill="none" stroke="currentColor"` (NO depender solo de CSS). Landing cards usan emojis (`.lc-emoji`), no ICO
- Profilaxis sin theory card, patron progresivo: categoria → procedimiento → prescripcion
- Micro usa sidebar-index con accordion + content panel
- `esc()` para todo texto dinamico. innerHTML solo con datos hardcoded
- Evitar abreviaturas en la UI (IAP → "Infeccion Protesica Articular", DAIR → "Lavado (DAIR)", S/I/R → sensible/intermedia/resistente en algoritmo)
- CSS: `.info-card>strong:first-child` para labels (no aplica a strong dentro de p)
- Responsive 480px: btn-home compacto, algoritmo 1 col con ramas dimmed ocultas (display:none), sir-grid 1 col, ls-input-grid 1 col
