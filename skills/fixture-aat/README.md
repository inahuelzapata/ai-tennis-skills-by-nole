# fixture-aat

## Version

fixture-aat v1

---

# Descripción

`fixture-aat` es una skill orientada a la extracción, curado y normalización de fixtures de Interclubes de tenis desde páginas de AAT / deportivoaat.

La skill toma una URL de AAT y devuelve:

- fixtures filtrados
- categorías relevantes
- nombres normalizados
- estructura organizada por día
- output estructurado para automatización
- vista humana para validación

La skill NO genera imágenes ni assets visuales.  
Su objetivo es funcionar como pipeline de datos para otras skills downstream.

---

# Estructura del Proyecto

```text
fixture-aat/
├── skill.md
├── README.md
├── examples/
│   ├── default.json
│   ├── custom-categories.json
│   └── edge-cases.json
├── mappings/
│   └── clubs.json
```

---

# Archivos Auxiliares

## examples/default.json

Ejemplo de uso default.

```json
{
  "url": "https://deportivoaat.com.ar/sis/app/vEntidad.php?idEntidad=214"
}
```

---

## examples/custom-categories.json

Ejemplo con categorías custom.

```json
{
  "url": "https://deportivoaat.com.ar/sis/app/vEntidad.php?idEntidad=214",
  "categories": [
    "DAMAS +30",
    "MIXTO LIBRE"
  ]
}
```

---

## examples/edge-cases.json

Casos especiales y edge cases contemplados por la skill.

Ejemplo:

```json
{
  "scenario": "same-category-same-time-different-team-variants",
  "fixtures": [
    {
      "category": "+19 - 4ta B",
      "time": "14:15",
      "match": "SOLANA A vs Deportes Racionales"
    },
    {
      "category": "+19 - 4ta B",
      "time": "14:15",
      "match": "SOLANA B vs Gazcon LTC"
    }
  ],
  "expected_behavior": "do_not_deduplicate"
}
```

---

## mappings/clubs.json

Mappings persistentes de nombres de clubes.

Ejemplo:

```json
{
  "ASOC. DEP. RACIONALES": "Deportes Racionales",
  "GAZCON LAWN TENNIS CLUB": "Gazcon LTC",
  "SOLANA TENIS CLUB A.CIVIL": "SOLANA"
}
```

---

# Scope

Esta skill está diseñada para alimentar sistemas downstream como:

- generadores de stories
- gráficos deportivos
- layouts editoriales
- pipelines de automatización
- overlays
- assets para redes sociales

La skill actúa exclusivamente como un pipeline de extracción y curado de datos.

---

# Inputs

## Required

### `url`

URL de AAT / deportivoaat a analizar.

Ejemplo:

```json
{
  "url": "https://deportivoaat.com.ar/sis/app/vEntidad.php?idEntidad=214"
}
```

---

# Configuración de Club

## `club_name`

Nombre canónico del club utilizado para detectar LOCAL / VISITANTE.

Default:

```text
SOLANA TENIS CLUB A.CIVIL
```

---

## `club_aliases`

Aliases válidos del club.

Default:

```json
[
  "Complejo Solana",
  "SOLANA",
  "Solana Tenis Club"
]
```

Todos los aliases deben considerarse equivalentes.

---

# Selección Default de Fechas

Por default, la skill analiza:

- el próximo sábado
- el próximo domingo

en base a la fecha actual del request.

Antes de finalizar, la skill debe preguntar si existe algún feriado adyacente antes o después del fin de semana.

Si el usuario lo confirma, el feriado debe incluirse en el rango analizado.

---

# Inputs Opcionales

## `date_range`

Permite definir fechas explícitas.

Ejemplo:

```json
{
  "start_date": "2026-05-23",
  "end_date": "2026-05-25"
}
```

---

## `categories`

Categorías custom a incluir.

Ejemplo:

```json
{
  "categories": [
    "CABALLEROS +19",
    "DAMAS +30",
    "MIXTO LIBRE"
  ]
}
```

---

## `club_name_map`

Mappings custom de clubes.

Ejemplo:

```json
{
  "club_name_map": {
    "ASOC. DEP. RACIONALES": "Deportes Racionales",
    "GAZCON LAWN TENNIS CLUB": "Gazcon LTC"
  }
}
```

Los mappings definidos por el usuario tienen prioridad absoluta.

---

# Categorías Default

Por default, la skill incluye:

## Caballeros — Interclubes por Edad

- +19
- +25
- +30

## Caballeros — Libres

- Tercera
- Cuarta
- Quinta

Salvo pedido explícito, la skill ignora:

- Damas
- Mixto
- +50
- juniors
- categorías irrelevantes

---

# Reminder Friendly

La skill debe recordarle amigablemente al usuario que pueden incluirse otras categorías.

Ejemplo:

```text
Actualmente filtrando:
- Caballeros +19 / +25 / +30
- Libres 3ra / 4ta / 5ta

También podés pedir:
- Damas
- Mixto
- +50
- otras divisiones
- categorías específicas
```

---

# Valid Fixture Rule

Una fila se considera válida únicamente si contiene:

- categoría
- fecha
- horario
- club local
- club visitante

Si alguno de estos datos falta:

- la skill NO debe inventarlo
- la skill NO debe inferirlo silenciosamente

Los fixtures incompletos deben enviarse a revisión.

---

# Fixture Extraction Rules

La skill debe extraer únicamente fixtures reales desde la página de AAT / deportivoaat.

La página de AAT es la única source of truth.

La skill NO debe inventar:

- rivales
- horarios
- fechas
- divisiones
- condición local / visitante

Si la página es ambigua, debe informarlo explícitamente y pedir confirmación.

---

# Manejo de Variantes de Equipo

La skill NO debe considerar variantes de equipo como duplicados.

Ejemplos:

- SOLANA A
- SOLANA B
- SOLANA C

representan entidades competitivas distintas.

---

# Duplicate Detection Rule

Para considerar un fixture como duplicado, deben coincidir:

- categoría
- división
- fecha
- horario
- club local
- club visitante
- variante de equipo

Si alguno cambia, NO debe deduplicarse.

---

# Regla LOCAL / VISITANTE

Si el club target aparece primero:

```text
SOLANA vs Club X
```

→ LOCAL

Si aparece segundo:

```text
Club X vs SOLANA
```

→ VISITANTE

---

# Normalización de Clubes

La skill debe limpiar nombres federativos para mejorar legibilidad y presentación.

El objetivo NO es reproducir el naming institucional exacto, sino producir nombres claros y utilizables.

---

# Cleanup Rules

Eliminar sufijos administrativos como:

```text
A.CIVIL
ASOCIACIÓN CIVIL
ASOC. CIVIL
ASOC.
```

Normalizar uppercase excesivo.

---

# Regla Lawn Tennis Club

Cualquier club que contenga:

```text
LAWN TENNIS CLUB
```

puede abreviarse como:

```text
LTC
```

Ejemplo:

```text
GAZCON LAWN TENNIS CLUB
→ Gazcon LTC
```

---

# Regla Tenis Club

Preservar `Tenis Club` si el nombre sigue siendo razonablemente corto.

Ejemplos válidos:

```text
El Prado Tenis Club
Oasis Tenis Club
```

Si el nombre es demasiado largo:

```text
Tenis Club → TC
```

Siempre priorizar:

- legibilidad
- limpieza visual
- identidad reconocible

---

# Display Name Formatting

La skill debe generar display names compactos.

Formato preferido:

```text
+19 - 4ta B
+30 - Segunda
+25 - Intermedia
Libres - Cuarta
```

Evitar naming federativo verboso.

---

# Fixture Card Content

Cada fixture debe exponer:

- `category_display`
- `time`
- `match_display`
- `condition`

Ejemplo:

```json
{
  "category_display": "Libres - Tercera",
  "time": "14:00",
  "match_display": "SOLANA vs Temperley LTC",
  "condition": "LOCAL"
}
```

---

# Structured Output

La skill debe devolver output estructurado agrupado por día.

Ejemplo:

```json
{
  "days": [
    {
      "date": "2026-05-24",
      "display_date": "DOMINGO 24/05",
      "fixtures": [
        {
          "category_display": "+19 - 4ta B",
          "time": "14:15",
          "match_display": "SOLANA vs Deportes Racionales",
          "condition": "LOCAL"
        }
      ]
    }
  ]
}
```

---

# Human Review Output

Antes de cualquier uso downstream, la skill debe mostrar una vista humana.

Ejemplo:

```text
SERIES ENCONTRADAS — FIN DE SEMANA

DOMINGO 24/05

• +19 - 4ta B
• 14:15 hs
• SOLANA vs Deportes Racionales
• LOCAL
```

---

# Fixtures Incompletos o Ambiguos

Si un fixture es ambiguo o incompleto, la skill debe pedir revisión explícitamente.

Ejemplo:

```text
Algunos fixtures no pudieron validarse completamente:

• +19 - 4ta B
• Rival faltante
• Domingo 24/05

¿Podés confirmar la información faltante?
```

La skill debe priorizar:

- precisión
- transparencia
- verificabilidad

por encima de intentar completar resultados a la fuerza.

---

# Final Confirmation Flow

Antes de finalizar el pipeline, la skill debe:

1. mostrar el output estructurado
2. mostrar la vista humana
3. pedir confirmación explícita

Ejemplo:

```text
¿Confirmás que use estas series?
```

Incluso si no existen ambigüedades, la confirmación sigue siendo obligatoria.

---

# Non Goals

Esta skill NO:

- genera imágenes
- genera stories
- genera gráficos
- aplica branding
- define tipografías
- renderiza layouts
- inventa fixtures
- asume datos faltantes
- ignora ambigüedades silenciosamente

La generación visual debe resolverse en otras skills downstream.

---

# Filosofía

`fixture-aat` separa:

- extracción
- curado
- normalización
- presentación
- renderizado visual

Esto permite construir pipelines reutilizables, consistentes y mantenibles para automatización deportiva.