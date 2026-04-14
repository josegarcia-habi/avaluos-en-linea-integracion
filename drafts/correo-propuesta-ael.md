# Correo para Avaluos en Linea — Propuesta de extensiones al spec FUA

**Para**: Contacto tecnico AEL
**De**: Jose Garcia — Tech Team HabiCapital
**Asunto**: Propuesta de extensiones al spec del servicio FUA — repo compartido

---

Hola equipo de Avaluos en Linea,

Desde HabiCapital hemos estado trabajando en la integracion automatizada del FUA via su API y hemos identificado algunos datos que el proceso manual por correo (Excel) siempre incluia pero que el spec actual de la API no contempla.

Para facilitar la colaboracion y tener trazabilidad de los cambios, creamos un repositorio publico en GitHub donde esta el spec OpenAPI actualizado:

**Repositorio**: https://github.com/josegarcia-habi/avaluos-en-linea-integracion

**Pull Request con la propuesta**: https://github.com/josegarcia-habi/avaluos-en-linea-integracion/pull/1

La idea del repo es que sea un espacio compartido donde cualquier cambio al spec se haga via Pull Request — asi ambos equipos revisamos el diff exacto antes de implementar.

## Resumen de los cambios propuestos

### 1. Fotos: 6 labels nuevos (ids 20-25)

El spec actual tiene 19 labels de fotos (ids 1-19). Proponemos 6 adicionales:

**Fachadas diferenciadas (ids 20-21)**: El proceso manual siempre incluia 3 fotos de fachada: la del conjunto desde afuera (id 1), la de la torre/edificio (nuevo id 20) y la entrada de la unidad (nuevo id 21). Con un solo label (`fachada_principal`) no se puede distinguir cual es cual. Los nombres propuestos (`fachada_exterior`, `fachada_edificio`, `fachada_inmueble`) son agnosticos al tipo de inmueble — aplican para apartamentos, casas, oficinas, locales.

**Interior general (id 22)**: Label con `allow_multiple: true` para fotos interiores que no encajan en categorias especificas (publicaciones del portal, estudios, balcones, zonas de ropas).

**Contadores individuales (ids 23-25)**: El label 17 (`instalaciones_mecanicas_medidores`) agrupa todo en uno solo. El proceso manual siempre pedia gas, luz y agua por separado porque el perito necesita verificar cada servicio. Proponemos `contador_gas` (23), `contador_luz` (24), `contador_agua` (25) como `required: true`. Se acepta foto del medidor o del recibo.

Tambien proponemos renombrar el label id 1 de `fachada_principal` a `fachada_exterior` para que sea descriptivo y no se confunda con los otros dos.

### 2. Contexto del edificio: 3 campos nuevos en `building_context`

El Excel manual incluia estas preguntas que la API no tiene:

- **`access_roads_condition`**: Estado de vias de acceso (`bueno`/`regular`/`malo`)
- **`neighbors_built`** + **`surrounding_constructions`**: Inmuebles vecinos construidos y tipo de construcciones (conjunto casas, edificios, locales comerciales, etc.)

### 3. Descripcion del edificio: `total_garages`

El Excel pedia "# de garajes del conjunto" junto con torres, aptos y pisos — que ya estan en el spec. Solo faltaba `total_garages` (total privados + visitantes).

## Como revisar

En el PR pueden ver el diff exacto del archivo `openapi/fua-service.yaml` — muestra linea por linea que se agrega. El `CHANGELOG.md` tiene la justificacion detallada de cada cambio.

Si les parece bien podemos agendar una llamada rapida para resolver dudas, o pueden comentar directamente en el PR.

Quedo atento,

Jose Garcia
Tech Team — HabiCapital
tech-team@habicapital.com
