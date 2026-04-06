# Rediseno Infecciones COT — Spec de Diseno

## Contexto

La herramienta actual usa pestanas horizontales que no son intuitivas para traumatologos poco familiarizados con apps clinicas. Se necesita un rediseno que:
- Sea mas intuitivo con una landing page tipo hub
- Incluya resumen teorico expandible en cada seccion
- Presente la secuencia IV/oral de forma visual con cards en cascada
- Redisene el espectro antibacteriano con estetica Sistema B coherente

## Decisiones de diseno

| Pregunta | Opcion elegida |
|----------|---------------|
| Layout principal | B: Landing page con cards tipo hub |
| Resumen teorico | C: Ficha rapida + "Leer mas" expandible |
| Secuencia IV/Oral | B: Cards en cascada con flechas |
| Espectro antibacteriano | B: Archivo separado rediseñado con Sistema B |

## 1. Landing Page

### Estructura
- Header Slate Steel con titulo, subtitulo, sky toggle, barra de alergias
- Grid de 6 cards (2 columnas desktop, 1 movil)
- Cada card: icono SVG + titulo + descripcion 1 linea + badge de contenido + color acento

### Las 6 cards
1. **Profilaxis Preoperatoria** — color sky — "Antibiotico antes de operar"
2. **Infeccion Protesica** — color violet — "IAP: DAIR, revision 1T y 2T"
3. **Infecciones Osteoarticulares** — color emerald — "Artritis septica, osteomielitis, espondilodiscitis"
4. **Partes Blandas** — color amber — "Celulitis, fascitis, herida quirurgica, mordeduras"
5. **Consulta por Microorganismo** — color red — "Buscar por germen del cultivo"
6. **Espectro Antibacteriano** — color slate — abre `espectro-antibac.html` en nueva pestana

### Comportamiento
- Clic en cards 1-5: transicion fadeIn a vista de seccion dentro de index.html
- Clic en card 6: `window.open('espectro-antibac.html')` (nueva pestana)
- Back-home button glass pill fijo en top-left al estar en una seccion

## 2. Vista de seccion

### Cabecera de seccion
- Barra con nombre de categoria + color acento + boton "Volver" a landing

### Resumen teorico expandible
- Glass card con borde izquierdo color acento
- Visible por defecto: 3-4 lineas (definicion, germenes frecuentes, principio terapeutico)
- Boton "Leer mas" que expande con transicion suave a mini-guia (clasificacion, criterios diagnosticos, algoritmo general)
- Icono de libro decorativo

### Contenido resumido por seccion

**Profilaxis:**
- Ficha: "La profilaxis antibiotica reduce la ISQ/IAP. Cefazolina 2g IV es el estandar. Timing: 30-60 min antes de incision."
- Expandido: indicaciones especiales (SARM, obesos >120kg, redosificacion), fracturas abiertas por Gustilo

**IAP:**
- Ficha: "La IAP afecta al 1-2% de artroplastias. S. aureus y S. epidermidis son los mas frecuentes. El tratamiento depende de aguda vs cronica."
- Expandido: clasificacion aguda/cronica, criterios DAIR, indicaciones revision 1T vs 2T, papel del biofilm
- Sub-navegacion: pills DAIR / Rev 1T / Rev 2T

**Osteoarticular:**
- Ficha: "Artritis septica es urgencia quirurgica. Osteomielitis requiere desbridamiento + antibioterapia prolongada."
- Expandido: clasificacion, diagnostico diferencial, indicaciones quirurgicas, criterios paso oral (OVIVA)
- Sub-navegacion: Artritis Septica / Osteomielitis Aguda / Cronica / Espondilodiscitis

**Partes Blandas:**
- Ficha: "Celulitis: S. aureus/Streptococcus. Fascitis necrotizante: emergencia quirurgica."
- Expandido: clasificacion gravedad, criterios ingreso, algoritmo celulitis purulenta vs no purulenta
- Sub-navegacion: Celulitis / Fascitis / Herida Qx / Mordeduras / Pie Diabetico

**Microorganismo:**
- Ficha: "Introduce el germen del cultivo para obtener recomendaciones de tratamiento IV, oral y duracion."
- Buscador neumorfico como entrada principal

### Selector de microorganismo
- Select agrupado por Gram+/Gram-/Anaerobios/Especiales (como ahora)
- Presente en secciones IAP, Osteoarticular

## 3. Secuencia terapeutica IV → Oral (cards en cascada)

### Estructura visual
Reemplaza las filas planas por 4 cards verticales conectadas:

1. **Card FASE IV** (fondo sky-light, borde izquierdo sky)
   - Icono hospital
   - Farmaco de eleccion + dosis
   - Alternativa IV
   - **Duracion IV** en negrita

2. **Pill CRITERIOS DE PASO** (fondo amber-light, centrado)
   - Criterios individualizados por microorganismo
   - Conectado con flecha SVG animada (pulso sutil)

3. **Card FASE ORAL** (fondo emerald-light, borde izquierdo emerald)
   - Icono pastilla
   - Farmaco oral + dosis
   - Alternativa oral

4. **Card DURACION TOTAL** (fondo violet-light, borde izquierdo violet)
   - Icono calendario
   - Duracion total por escenario

### Flechas conectoras
- SVG inline con path vertical
- Color: `var(--text-light)`
- Animacion: `pulse` sutil (opacidad 0.5 → 1) en `prefers-reduced-motion: no-preference`
- Punta de flecha triangular

### Debajo de las cards
- Seccion **Notas** (glass card, mismo estilo que ahora)
- Seccion **Referencias** con ref-tags

## 4. Espectro antibacteriano (espectro-antibac.html)

### Rediseno visual
- Header Slate Steel identico al index.html
- Back-home button → landing Infecciones (href="index.html")
- Fuentes: Source Serif 4 / Source Sans 3 / Source Code Pro
- Fondo: `var(--bg)` del Sistema B
- Dark mode con sky toggle y variables `[data-theme="dark"]`
- Buscador neumorfico (mismo estilo que en index.html)

### Colores de grupos
| Grupo | Actual | Nuevo |
|-------|--------|-------|
| Gram+ | `#2196f3` neon | `var(--sky)` #0284c7 |
| Gram- | `#4caf50` neon | `var(--emerald)` #10b981 |
| Anaerobios | `#e91e8c` | `var(--violet)` #7c3aed |
| Especiales | `#9c27b0` | `var(--amber)` #f59e0b |

### Celdas de actividad
- Bordes redondeados (radius-sm)
- Gradientes mas suaves usando paleta Sistema B
- Tooltips con glass card en vez de fondo dark

### Datos
- Mismos 42 antibioticos y 15 microorganismos
- Actualizar texto header a "2025 · Actualizado"
- Logica de filtrado y tooltip se mantiene

## 5. Archivos a modificar

| Archivo | Accion |
|---------|--------|
| `index.html` | Reescribir completo con landing + secciones + cascada |
| `espectro-antibac.html` | Reescribir visual con Sistema B, mantener datos |

## 6. Datos clinicos

Se mantienen todos los datos ya implementados:
- PROPHYLAXIS (11 procedimientos)
- ORGANISMS (18 microorganismos con ivDur, pasoOral)
- OA_INFO (4 tipos)
- PB_DATA (5 tipos)
- Resumenes teoricos: nuevos textos para cada seccion

## 7. Verificacion

1. Abrir index.html — ver landing con 6 cards
2. Clic en cada card — ver seccion con resumen expandible
3. Seleccionar microorganismo — ver cascada IV→Oral con flechas
4. Activar alergias — verificar que cascada se actualiza
5. Abrir espectro — verificar estetica Sistema B coherente
6. Dark mode en ambos archivos
7. Responsive movil (375px) — grid 1 columna, cascada vertical OK
8. Boton volver funciona en todas las secciones
