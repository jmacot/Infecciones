# Infecciones COT

Guia de antibioterapia en infecciones musculoesqueleticas para cirujanos ortopedicos.

Herramienta clinica single-file HTML diseñada para consultar rapidamente el tratamiento antibiotico adecuado segun el microorganismo aislado, el escenario clinico y las alergias del paciente.

## Contenido

### Secciones clinicas

- **Infeccion Protesica Articular** — Lavado (DAIR), revision en 1 y 2 tiempos. Tratamiento empirico y dirigido por microorganismo. Checklist de toma de muestras intraoperatorias.
- **Infecciones Osteoarticulares** — Artritis septica, osteomielitis aguda y cronica, espondilodiscitis piogena. Tratamiento empirico y dirigido.
- **Partes Blandas** — Celulitis, fascitis necrotizante, infeccion de herida quirurgica, mordeduras, pie diabetico.

### Herramientas de consulta

- **Profilaxis Preoperatoria** — 11 procedimientos COT con farmaco, dosis, redosificacion y alternativas. Factores de riesgo modificables (ICM 2025).
- **Consulta por Microorganismo** — 18 fichas (8 Gram+, 6 Gram-, 1 anaerobio, 3 especiales) con secuencia IV → paso oral → duracion total adaptada por escenario clinico.
- **Espectro Antibacteriano** — Tabla interactiva de 42 antibioticos x 15 microorganismos con filtros por familia, germen y actividad.

## Funcionalidades

- **Filtro de alergias** — Betalactamicos, quinolonas, vancomicina. Modifica automaticamente todas las recomendaciones.
- **Tratamiento empirico** — Visible en cada seccion antes de conocer el cultivo.
- **Secuencia terapeutica visual** — Cards en cascada IV → Criterios de paso → Oral → Duracion total con flechas animadas.
- **Resumen teorico expandible** — En cada seccion, con dosis resaltadas y listas estructuradas.
- **Referencias clicables** — Enlaces directos a PRIOAM, Hospital Macarena, IDSA, OVIVA segun patologia.
- **Dark mode** — Toggle automatico por hora y manual.
- **Responsive** — Diseñado para consultar desde el movil en planta o quirofano.

## Fuentes

- **Tercer Consenso Internacional de Infecciones Musculoesqueleticas (ICM 2025, Estambul)** — Parvizi & Gehrke. Fuente principal.
- **Guia PRIOAM** — guiaprioam.com — Terapia antimicrobiana del Area Aljarafe (Sevilla).
- **Protocolo Hospital Virgen Macarena** — antibioterapia.hospitalmacarena.es
- **IDSA PJI Guidelines** — Osmon et al., 2013.
- **Ensayo OVIVA** — Li et al., NEJM 2019. Paso precoz a oral en infecciones oseas.

## Estructura

```
Infecciones/
├── index.html              # App principal (single-file)
├── espectro-antibac.html   # Tabla de espectro antibacteriano
├── fuentes/                # PDFs de referencia
├── docs/                   # Specs de diseño
└── README.md
```

## Diseño

Sistema B "Radiologia Glass" del ecosistema COT:
- Fuentes: Source Serif 4 + Source Sans 3 + Source Code Pro
- Glassmorphism con backdrop-filter
- Iconos SVG stroke-based (Lucide-style)
- Paleta: sky, violet, emerald, amber, red

## Autor

**jmacot** — Cirugia Ortopedica y Traumatologia, Sevilla.

## Aviso

Herramienta orientativa. No sustituye el criterio clinico, el antibiograma local ni la consulta con Enfermedades Infecciosas.

## Licencia

MIT
