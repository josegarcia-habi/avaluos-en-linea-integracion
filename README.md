# Avaluos en Linea - Integracion HabiCapital

Documentacion tecnica compartida de la integracion entre **HabiCapital** y **Avaluos en Linea (AEL)** para la generacion del Formato Unico de Avaluo (FUA).

## Contenido

| Archivo | Descripcion |
|---------|-------------|
| `openapi/fua-service.yaml` | OpenAPI spec del servicio FUA |
| `examples/ejemplo-curl.txt` | Ejemplo completo de request (curl) |
| `examples/webhook-response.json` | Ejemplo de respuesta del webhook |
| `CHANGELOG.md` | Historial de cambios al spec |

## OpenAPI Spec

El archivo `openapi/fua-service.yaml` es la **fuente de verdad** del contrato de la API. Cualquier cambio al spec debe hacerse via Pull Request para que ambas partes revisen el diff antes de implementar.

### Flujo de cambios

1. Crear un branch desde `main`
2. Modificar el spec en `openapi/fua-service.yaml`
3. Documentar el cambio en `CHANGELOG.md`
4. Abrir un Pull Request con la justificacion del cambio
5. Ambas partes revisan y aprueban
6. Merge a `main` = cambio acordado

### Visualizar el spec

El spec se puede visualizar con cualquier herramienta compatible con OpenAPI 3.0:

- [Swagger Editor](https://editor.swagger.io/) — pegar el contenido del YAML
- [Redocly](https://redocly.github.io/redoc/) — renderizado de documentacion

## Contacto

| Equipo | Contacto |
|--------|----------|
| HabiCapital Tech | tech-team@habicapital.com |
| Avaluos en Linea | (por definir) |
