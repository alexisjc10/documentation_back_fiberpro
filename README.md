# FiberPro_External_Query_Back
Consultas de usuarios externos a Odoo.

## Documentación de las APIS

### Blueprint `entrando`

El blueprint `entrando` maneja los endpoints de autenticación de usuarios en la aplicación Flask. A continuación se detallan las rutas disponibles y su funcionalidad.

#### Prefijo de URL
Todas las rutas bajo este blueprint están prefijadas con `/api/entrando`.

### Rutas

#### Iniciar Sesión

**Endpoint:** `/api/entrando/login/`  
**Método:** `POST`  
**Descripción:** Autentica a un usuario utilizando su nombre de usuario y contraseña. Si las credenciales son correctas, el usuario inicia sesión.

**Solicitud:**
- **Encabezados:** `Content-Type: application/json`
- **Cuerpo:** Objeto JSON que contiene los siguientes campos:
  - `username`: (string) El nombre de usuario del usuario.
  - `password`: (string) La contraseña del usuario.

**Respuesta:**
- **Éxito (200):** Devuelve un objeto JSON con un mensaje de éxito y el tipo de usuario.
```json
  {
    "message": "Login successful",
    "type": "user_type"
  }
```
- **Error del Cliente (400):** Devuelve un objeto JSON indicando que el nombre de usuario y la contraseña son obligatorios.
```json
  {
  "message": "Username and password are required"
  }
```
- **No Autorizado (401):** Devuelve un objeto JSON indicando credenciales inválidas.
```json
  {
  "message": "Invalid username or password"
  }
```
*****************************************************************************************

### Blueprint `invoice_table`

El blueprint `invoice_table` maneja los endpoints relacionados con la obtención de facturas de socios en la aplicación Flask. A continuación se detallan las rutas disponibles y su funcionalidad.

#### Prefijo de URL
Todas las rutas bajo este blueprint están prefijadas con `/api/invoice_table`.

### Rutas

#### Obtener Facturas de Socios

**Endpoint:** `/api/invoice_table/<string:subscription_id>`  
**Método:** `GET`  
**Descripción:** Obtiene las facturas asociadas a una suscripción específica basada en el `subscription_id`.

**Parámetros de URL:**
- `subscription_id`: (string) El ID de la suscripción para la cual se desean obtener las facturas.

**Respuesta:**
- **Éxito (200):** Devuelve un array de objetos JSON, cada uno representando una factura con los siguientes campos:
  - `create_date`: (string) La fecha de creación de la factura.
  - `x_studio_nro_de_documento`: (string) El número de documento de la factura.
  - `partner_id`: (string) El nombre del socio asociado a la factura, o 'Sin Dato' si no está disponible.
  - `invoice_date`: (string) La fecha de la factura.
  - `x_studio_fecha_inicio_suscripcion`: (string) La fecha de inicio de la suscripción.
  - `amount_total_signed`: (float) El monto total firmado de la factura.
  - `amount_residual_signed`: (float) El monto residual firmado de la factura.
  - `state`: (string) El estado de la factura, traducido a términos comprensibles ('Publicado', 'Cancelado', 'Borrador').
  - `payment_state`: (string) El estado de pago de la factura, traducido a términos comprensibles ('No Pagado', 'Pagado', 'En Proceso de Pago', 'Parcialmente Pagado', 'Revertido', 'Factura Sistema Anterior').
  - `x_studio_obs_suscri`: (string) Observaciones de la suscripción.
  - `x_studio_nro_suscripcion`: (string) Número de suscripción.
  - `x_studio_producto`: (string) Producto asociado a la suscripción.
  - `x_studio_fecha_suspension`: (string) Fecha de suspensión de la suscripción.
  - `x_studio_close_reason_id`: (string) Razón de cierre de la suscripción.
  - `x_studio_fecha_activacion`: (string) Fecha de activación de la suscripción.
  - `narration`: (string) Narración de la factura.
  - `x_studio_reclamo_dato`: (string) Datos de reclamo asociados a la factura.

**Ejemplo de Solicitud:**
```bash
curl -X GET http://localhost:5000/api/invoice_table/12345 \
-H "Content-Type: application/json"
```
**Ejemplo de Respuesta:**
```json
[
  {
    "create_date": "2023-05-01 12:34:56",
    "x_studio_nro_de_documento": "INV/2023/001",
    "partner_id": "Empresa XYZ",
    "invoice_date": "2023-05-01",
    "x_studio_fecha_inicio_suscripcion": "2023-04-01",
    "amount_total_signed": 100.0,
    "amount_residual_signed": 50.0,
    "state": "Publicado",
    "payment_state": "No Pagado",
    "x_studio_obs_suscri": "Observación 1",
    "x_studio_nro_suscripcion": "SUB/2023/001",
    "x_studio_producto": "Producto A",
    "x_studio_fecha_suspension": "2023-06-01",
    "x_studio_close_reason_id": "Razón X",
    "x_studio_fecha_activacion": "2023-04-01",
    "narration": "Narración de la factura",
    "x_studio_reclamo_dato": "Reclamo Y"
  }
]
```
*****************************************************************************************

### Blueprint `res_partner`

El blueprint `res_partner` maneja los endpoints relacionados con la obtención de direcciones de socios en la aplicación Flask. A continuación se detallan las rutas disponibles y su funcionalidad.

#### Prefijo de URL
Todas las rutas bajo este blueprint están prefijadas con `/api/res_partner`.

### Rutas

#### Obtener Direcciones de Socios

**Endpoint:** `/api/res_partner/<string:usuario>`  
**Método:** `GET`  
**Descripción:** Obtiene las direcciones asociadas a un socio específico basado en el `usuario`.

**Parámetros de URL:**
- `usuario`: (string) El identificador de usuario (por ejemplo, número de VAT) del socio para el cual se desean obtener las direcciones.

**Respuesta:**
- **Éxito (200):** Devuelve un array de objetos JSON, cada uno representando una dirección con los siguientes campos:
  - `name`: (string) El nombre del socio.
  - `id`: (int) El ID del socio.

**Ejemplo de Solicitud:**
```bash
curl -X GET http://localhost:5000/api/res_partner/123456789 \
-H "Content-Type: application/json"
```
**Ejemplo de Respuesta:**
```json
[
  {
    "name": "John Doe",
    "id": 1
  },
  {
    "name": "Jane Smith",
    "id": 2
  }
]

```
*****************************************************************************************

### Blueprint `subscription`

El blueprint `subscription` maneja los endpoints relacionados con la obtención de suscripciones de ventas en la aplicación Flask. A continuación se detallan las rutas disponibles y su funcionalidad.

#### Prefijo de URL
Todas las rutas bajo este blueprint están prefijadas con `/api/sale_subscriptions`.

### Rutas

#### Obtener Suscripciones de Ventas por Socio

**Endpoint:** `/api/sale_subscriptions/<string:usuario>`  
**Método:** `GET`  
**Descripción:** Obtiene las suscripciones de ventas asociadas a un socio específico basado en el `usuario`.

**Parámetros de URL:**
- `usuario`: (string) El número de documento (por ejemplo, número de VAT) del socio para el cual se desean obtener las suscripciones de ventas.

**Respuesta:**
- **Éxito (200):** Devuelve un array de objetos JSON, cada uno representando una suscripción con los siguientes campos:
  - `display_name`: (string) El nombre de la suscripción, combinado con el nombre de la etapa.
  - `stage_id`: (string) El nombre de la etapa de la suscripción.
  - `id`: (int) El ID de la suscripción.

**Ejemplo de Solicitud:**
```bash
curl -X GET http://localhost:5000/api/sale_subscriptions/123456789 \
-H "Content-Type: application/json"
```
**Ejemplo de Respuesta:**
```json
[
  {
    "display_name": "Suscripción 1",
    "stage_id": "Activo",
    "id": 1
  },
  {
    "display_name": "Suscripción 2",
    "stage_id": "Inactivo",
    "id": 2
  }
]
```
*****************************************************************************************

### Blueprint `tickets`

El blueprint `tickets` maneja los endpoints relacionados con la obtención de tickets de soporte en la aplicación Flask. A continuación se detallan las rutas disponibles y su funcionalidad.

#### Prefijo de URL
Todas las rutas bajo este blueprint están prefijadas con `/api/helpdesk_ticket`.

### Rutas

#### Obtener Tickets de Soporte por Dirección

**Endpoint:** `/api/helpdesk_ticket/<string:address_id>`  
**Método:** `GET`  
**Descripción:** Obtiene los tickets de soporte asociados a una dirección específica basada en el `address_id`.

**Parámetros de URL:**
- `address_id`: (string) El ID de la dirección para la cual se desean obtener los tickets de soporte.

**Respuesta:**
- **Éxito (200):** Devuelve un array de objetos JSON, cada uno representando un ticket de soporte con los siguientes campos:
  - `team_id`: (string) El equipo de soporte asignado al ticket, o 'Sin Dato' si no está disponible.
  - `x_studio_nro_de_documento`: (string) El número de documento asociado al ticket.
  - `partner_id`: (string) El socio asociado al ticket, o 'Sin Dato' si no está disponible.
  - `x_studio_nombre_direccion`: (string) El nombre de la dirección asociada al ticket.
  - `x_studio_canal_id`: (string) El canal de origen del ticket, o 'Sin Dato' si no está disponible.
  - `create_date`: (string) La fecha de creación del ticket.
  - `x_studio_nro_de_ticket`: (string) El número del ticket.
  - `x_studio_nro_ticket_vt`: (string) El número de ticket de VT.
  - `x_studio_nro_ticket_reclamo`: (string) El número de ticket de reclamo.
  - `ticket_type_id`: (string) El tipo de ticket.
  - `x_studio_diagnostico_aacc_id`: (string) El diagnóstico AACC, o 'Sin Dato' si no está disponible.
  - `x_studio_observacion_aacc`: (string) Las observaciones AACC.
  - `x_studio_tecnico_noc_id`: (string) El técnico NOC asignado al ticket, o 'Sin Dato' si no está disponible.
  - `x_studio_diagnostico_noc_id`: (string) El diagnóstico NOC, o 'Sin Dato' si no está disponible.
  - `x_studio_solucion_noc_id`: (string) La solución NOC, o 'Sin Dato' si no está disponible.
  - `x_studio_observacion_noc`: (string) Las observaciones NOC.
  - `x_studio_tipos_de_reclamo`: (string) Los tipos de reclamo asociados al ticket.
  - `x_studio_codigo_sar`: (string) El código SAR asociado al ticket.
  - `x_studio_fecha_sar`: (string) La fecha del SAR.
  - `x_studio_codigo_de_apelacin`: (string) El código de apelación asociado al ticket.
  - `x_studio_codigo_sara`: (string) El código SARA asociado al ticket.

**Ejemplo de Solicitud:**
```bash
curl -X GET http://localhost:5000/api/helpdesk_ticket/12345 \
-H "Content-Type: application/json"
```
**Ejemplo de Respuesta:**
```json
[
  {
    "team_id": "Soporte Nivel 1",
    "x_studio_nro_de_documento": "DOC123",
    "partner_id": "Empresa XYZ",
    "x_studio_nombre_direccion": "Dirección ABC",
    "x_studio_canal_id": "Correo Electrónico",
    "create_date": "2023-05-01 12:34:56",
    "x_studio_nro_de_ticket": "TICKET123",
    "x_studio_nro_ticket_vt": "VT123",
    "x_studio_nro_ticket_reclamo": "RECLAMO123",
    "ticket_type_id": "Tipo A",
    "x_studio_diagnostico_aacc_id": "Diagnóstico AACC",
    "x_studio_observacion_aacc": "Observación AACC",
    "x_studio_tecnico_noc_id": "Técnico NOC",
    "x_studio_diagnostico_noc_id": "Diagnóstico NOC",
    "x_studio_solucion_noc_id": "Solución NOC",
    "x_studio_observacion_noc": "Observación NOC",
    "x_studio_tipos_de_reclamo": "Tipo de Reclamo",
    "x_studio_codigo_sar": "SAR123",
    "x_studio_fecha_sar": "2023-05-01",
    "x_studio_codigo_de_apelacin": "Apelación123",
    "x_studio_codigo_sara": "SARA123"
  }
]
```
*****************************************************************************************
### Blueprint `warehouse`

El blueprint `warehouse` maneja los endpoints relacionados con la obtención de información de stock en la aplicación Flask. A continuación se detallan las rutas disponibles y su funcionalidad.

#### Prefijo de URL
Todas las rutas bajo este blueprint están prefijadas con `/api/warehouse`.

### Rutas

#### Obtener Stock por Ubicación

**Endpoint:** `/api/warehouse/<string:location_id>`  
**Método:** `GET`  
**Descripción:** Obtiene la información de stock asociada a una ubicación específica basada en el `location_id`.

**Parámetros de URL:**
- `location_id`: (string) El ID de la ubicación para la cual se desean obtener los detalles de stock.

**Respuesta:**
- **Éxito (200):** Devuelve un array de objetos JSON, cada uno representando un artículo en stock con los siguientes campos:
  - `product_id`: (string) El ID del producto, o 'Sin Dato' si no está disponible.
  - `x_studio_related_field_Beb48`: (string) El nombre del producto, o 'Sin Dato' si no está disponible.
  - `x_studio_tipo_de_producto`: (string) El tipo de producto, o 'Sin Dato' si no está disponible.
  - `x_studio_categora_de_producto`: (string) La categoría del producto.
  - `location_id`: (string) El ID de la ubicación del stock, o 'Sin Dato' si no está disponible.
  - `lot_id`: (string) El ID del lote, o 'Sin Dato' si no está disponible.
  - `available_quantity`: (float) La cantidad disponible en stock.
  - `quantity`: (float) La cantidad total en stock.

**Ejemplo de Solicitud:**
```bash
curl -X GET http://localhost:5000/api/warehouse/12345 \
-H "Content-Type: application/json"
```
**Ejemplo de Respuesta:**
```json
[
  {
    "product_id": "Producto 1",
    "x_studio_related_field_Beb48": "Nombre Producto 1",
    "x_studio_tipo_de_producto": "Tipo Producto 1",
    "x_studio_categora_de_producto": "Categoría 1",
    "location_id": "Ubicación 1",
    "lot_id": "Lote 1",
    "available_quantity": 100,
    "quantity": 150
  },
  {
    "product_id": "Producto 2",
    "x_studio_related_field_Beb48": "Nombre Producto 2",
    "x_studio_tipo_de_producto": "Tipo Producto 2",
    "x_studio_categora_de_producto": "Categoría 2",
    "location_id": "Ubicación 1",
    "lot_id": "Lote 2",
    "available_quantity": 50,
    "quantity": 80
  }
]

```
