# Changelog

Historial de cambios al spec de la API FUA.

## [v2.1.0] - 2026-04-06 (PROPUESTA — pendiente aprobacion AEL)

Cambios propuestos por HabiCapital para cubrir toda la informacion que el proceso manual por correo (Excel) siempre incluia y que la API v2 no contemplaba. Detalle en el PR.

### 1. Renombrar `fachada_principal` a `fachada_exterior` (label id 1)

**Cambio**: El label id 1 pasa de llamarse `fachada_principal` a `fachada_exterior`.

**Justificacion**: El nombre original es ambiguo — "principal" no describe que es lo que se ve. La foto id 1 siempre ha sido la vista del conjunto/edificio/casa **desde la calle**. El nombre `fachada_exterior` es auto-descriptivo y no se confunde con los otros dos labels de fachada que proponemos (edificio e inmueble). Ademas, aplica igual para apartamentos, casas, oficinas, locales y cualquier otro tipo de inmueble sin necesidad de cambiar el nombre.

### 2. Nuevos photo labels (ids 20-25)

El spec v2 original tiene 19 labels (ids 1-19). Proponemos 6 labels adicionales para cubrir categorias que el proceso manual siempre incluia pero que no tenian label dedicado.

#### 2a. Labels de fachada especificos (ids 20-21)

| ID | Nombre | Descripcion |
|----|--------|-------------|
| 20 | `fachada_edificio` | Fachada de la torre, bloque o edificio especifico dentro de un conjunto |
| 21 | `fachada_inmueble` | Entrada de la unidad: puerta del apartamento, frente de la casa, acceso del local |

**Justificacion**: El proceso manual siempre incluia 3 fotos de fachada diferenciadas: (1) el conjunto desde afuera, (2) la torre/edificio especifico, (3) la entrada de la unidad. El spec v2 solo tiene `fachada_principal` (id 1) que es ambiguo. Con 3 labels especificos, el perito sabe exactamente que ve en cada foto sin depender de que se suban en orden. Estos nombres son **agnosticos al tipo de inmueble** — `fachada_inmueble` aplica igual para un apartamento (puerta del apto), una casa (frente de la casa), una oficina (entrada de la oficina) o un local comercial.

#### 2b. Label interior general (id 22)

| ID | Nombre | allow_multiple | Descripcion |
|----|--------|----------------|-------------|
| 22 | `interior_general` | **true** | Fotos generales del interior sin clasificacion especifica |

**Justificacion**: Muchas fotos del inmueble no encajan en las categorias especificas (sala, cocina, etc.) — por ejemplo, fotos de publicacion del portal inmobiliario, estudios, balcones, zonas de ropas. En vez de crear un label por cada una (que seria rigido), un label generico con `allow_multiple: true` permite enviar N fotos adicionales del interior sin perder las que si tienen label especifico. El label 19 (`otros`) existe pero es demasiado generico — mezcla interior con exterior. Este label es especificamente para interiores.

#### 2c. Labels de contadores de servicios (ids 23-25)

| ID | Nombre | required | Descripcion |
|----|--------|----------|-------------|
| 23 | `contador_gas` | **true** | Foto del contador de gas o recibo de gas |
| 24 | `contador_luz` | **true** | Foto del contador de luz o recibo de energia |
| 25 | `contador_agua` | **true** | Foto del contador de agua o recibo de acueducto |

**Justificacion**: El spec v2 tiene `instalaciones_mecanicas_medidores` (id 17) que agrupa todos los medidores en un solo label. El problema: el proceso manual **siempre** pedia los 3 contadores por separado (gas, luz, agua) porque el perito necesita verificar cada servicio independientemente. Con un solo label es imposible saber si la foto es del gas, la luz o el agua. Proponemos 3 labels individuales marcados como `required: true` porque son obligatorios para el FUA. Se acepta foto del contador fisico **o** del recibo del servicio (algunos inmuebles tienen medidor compartido o en zona inaccesible).

### 3. Campos nuevos en `BuildingContextRequest`

#### 3a. `access_roads_condition` (enum)

```yaml
access_roads_condition:
  type: string
  enum: [bueno, regular, malo]
  nullable: true
```

**Justificacion**: El Excel manual siempre incluia "Estado de vias de acceso" como campo obligatorio. Es informacion que el perito usa para evaluar accesibilidad del inmueble y que afecta el valor del avaluo. Los valores (`bueno`, `regular`, `malo`) son los mismos que usa nuestro sistema de captura de datos del conjunto.

#### 3b. `neighbors_built` (boolean) + `surrounding_constructions` (enum array)

```yaml
neighbors_built:
  type: boolean
  nullable: true

surrounding_constructions:
  type: array
  items:
    enum: [conjunto_casas, conjunto_edificios, casas_independientes, locales_comerciales, lotes_baldios]
  nullable: true
```

**Justificacion**: El Excel manual incluia dos preguntas relacionadas: "Inmuebles vecinos construidos? (si/no)" y "Tipo de construcciones (PH, locales, casas)". El perito usa esta informacion para evaluar el entorno urbano y las construcciones aledanas. `neighbors_built` es un boolean derivado (true si hay al menos una construccion vecina) y `surrounding_constructions` detalla los tipos. Los valores del enum reflejan las opciones que operadores capturan en campo.

### 4. Campo nuevo en `BuildingDescriptionRequest`

#### `total_garages` (integer)

```yaml
total_garages:
  type: integer
  description: "Total garages in the complex (private + visitor)"
```

**Justificacion**: El Excel manual incluia "# de garajes" en la seccion "Datos del conjunto" junto con # de torres, # de aptos/casas, y # de pisos (que ya estan en el spec como `total_tower_count`, `total_units_count`, `total_building_floors`). `total_garages` es el unico que faltaba. Representa el total de parqueaderos del conjunto (privados + visitantes), complementando el detalle individual que ya se envia en `property_units[]`.

### 5. Nuevos enum schemas

Se agregan los schemas `AccessRoadsConditionEnum`, `SurroundingConstructionEnum` y `SurroundingConstructionsReference` como tablas de referencia para los nuevos campos.

---

## [v2.0.0] - 2026-03-17

Spec v2 inicial entregado por Avaluos en Linea. Incluye:

- Endpoint `POST /` para crear FUA
- Endpoint `GET /{reference}` para consultar FUA
- Webhook de notificacion
- 19 photo labels (ids 1-19)
- 30 amenidades comunes
- 14 subtipos de inmueble
- Soporte para owners, property_units, property_legal
