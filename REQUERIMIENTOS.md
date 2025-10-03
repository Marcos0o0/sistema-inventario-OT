# REQUERIMIENTOS DEL SISTEMA

## 1. Descripción del Cliente y Problema Principal

El cliente es una pequeña empresa que maneja un negocio de reparaciones a vehículos y venta de repuestos automotrices. Actualmente, el dueño lleva un registro manual de inventarios y de las órdenes de trabajo que realiza. El problema principal es que este proceso manual es propenso a errores, consume mucho tiempo y no permite un control eficiente sobre el inventario ni las órdenes de trabajo.

La necesidad es crear un sistema digital que automatice estos registros y permita un control más preciso y rápido tanto del inventario como de las órdenes de trabajo.

## 2. Plazo y Alcance del Proyecto

### Plazo de Desarrollo:
**4 semanas** para completar el MVP funcional.

### Alcance del Proyecto:

**Incluido en el MVP:**
- Sistema de autenticación con JWT.
- Gestión completa de productos (crear, editar, eliminar, listar, buscar).
- Gestión completa de clientes (crear, editar, eliminar, listar, buscar).
- Gestión de órdenes de trabajo (crear, listar, cambiar estado).
- Asignación de productos a órdenes con descuento automático de stock.
- Registro automático de movimientos de inventario.
- Validaciones de reglas de negocio.
- Base de datos MongoDB.
- Cache con Redis para productos.
- Proyecto dockerizado con documentación.

**Excluido del MVP (versiones futuras):**
- Interfaz gráfica web o móvil.
- Roles adicionales (Cliente, Vendedor).
- Reportes y dashboards con gráficos.
- Sistema de alertas y notificaciones.
- Gestión de proveedores.
- Sistema de facturación.
- Backup automático y exportación de datos.

### Criterios de Éxito:
- El sistema permite gestionar productos, clientes y órdenes de forma completa.
- El stock se actualiza automáticamente al asignar productos a órdenes.
- Todas las reglas de negocio se validan correctamente.
- El proyecto se ejecuta exitosamente con docker-compose.
- La documentación permite a cualquier desarrollador levantar el proyecto.

## 3. Lista de Usuarios del Sistema

- Administrador

## 4. Usuarios y Permisos según Rol

**Administrador:**
- Crear, editar y eliminar productos.
- Gestionar el inventario y crear órdenes de trabajo.
- Ver historial de órdenes y reportes financieros.
- Gestionar clientes del sistema.
- Asignar productos a órdenes de trabajo.

## 5. Funciones Indispensables por Perfil

**Administrador (orden de prioridad):**
1. Autenticación y acceso seguro al sistema.
2. Gestión completa de productos.
3. Gestión de órdenes de trabajo.
4. Gestión de clientes.
5. Control de inventario y stock de productos.
6. Visualización de historial de órdenes.
7. Búsqueda y filtrado de productos, órdenes y clientes.

## 6. Datos Básicos a Almacenar

### Producto:
- ID
- Nombre
- Precio (CLP)
- Stock
- Categoría
- Descripción
- Proveedor
- Fecha de ingreso

### Cliente:
- ID
- Nombre completo
- Teléfono
- Correo electrónico (opcional)
- Fecha de registro

### Orden de Trabajo:
- ID
- ID Cliente (referencia)
- Fecha de emisión
- Descripción del trabajo
- Vehículo (Marca, Modelo, Año, Patente)
- Kilometraje del vehículo
- Estado (Pendiente, En progreso, Completada)
- Fecha de entrega estimada
- Fecha de finalización
- Precio total (CLP)

### Detalle de Orden:
- ID
- ID Orden (referencia)
- ID Producto (referencia)
- Cantidad utilizada
- Precio unitario al momento de la orden

### Movimientos de Inventario:
- ID
- ID Producto (referencia)
- Cantidad
- Tipo de movimiento (Entrada, Salida, Ajuste)
- Fecha de movimiento
- Descripción

## 7. Reglas del Negocio

### 7.1. Usuarios y Roles
- Solo existe el rol de Administrador con acceso completo al sistema.
- El acceso requiere autenticación mediante credenciales válidas con JWT.

### 7.2. Productos e Inventario
- El precio de los productos debe ser mayor a cero.
- El stock no puede ser negativo en ninguna circunstancia.
- Al asignar productos a una orden, el stock se descuenta automáticamente.
- No se puede asignar más cantidad de producto que el stock disponible.
- Toda modificación de stock debe quedar registrada en Movimientos de Inventario.

### 7.3. Clientes
- Todas las órdenes de trabajo deben estar asociadas a un cliente registrado.
- Los datos obligatorios del cliente son: nombre completo y teléfono.
- No se puede eliminar un cliente que tenga órdenes de trabajo asociadas.

### 7.4. Órdenes de Trabajo
- Los estados posibles son: Pendiente, En progreso, Completada.
- Toda orden nueva se crea en estado Pendiente.
- Solo el Administrador puede cambiar el estado de una orden.
- No se pueden eliminar órdenes en estado En progreso o Completada.
- El precio total de la orden debe ser mayor a cero.
- La fecha de finalización no puede ser anterior a la fecha de emisión.
- Los datos del vehículo (marca, modelo, año, patente) son obligatorios.

### 7.5. Detalle de Órdenes
- Cada producto asignado a una orden debe registrarse en Detalle de Orden.
- Se debe guardar el precio unitario del producto al momento de asignarlo.
- La cantidad asignada debe ser mayor a cero.
- No se puede asignar un producto con stock insuficiente.

### 7.6. Movimientos de Inventario
- Todo cambio de stock debe registrarse con tipo, cantidad, fecha y descripción.
- Los tipos de movimiento válidos son: Entrada, Salida, Ajuste.
- No se permiten movimientos que generen stock negativo.

### 7.7. Seguridad y Persistencia
- El acceso se controla mediante autenticación JWT con tiempo de expiración.
- Las contraseñas deben almacenarse encriptadas.
- La información se guarda en MongoDB.
- Redis se utiliza para cachear la lista de productos.

### 7.8. Usabilidad
- La interfaz API debe tener respuestas claras y descriptivas.
- Se deben retornar códigos HTTP apropiados para cada operación.
- Las respuestas del sistema deben ser rápidas (menos de 2 segundos).

## 8. Requisitos Funcionales y No Funcionales

### Requisitos Funcionales:

**Gestión de Productos:**
- Registrar, editar, eliminar y listar productos.
- Buscar productos por nombre o categoría.

**Gestión de Clientes:**
- Registrar, editar, eliminar y listar clientes.
- Buscar clientes por nombre o teléfono.
- Mostrar historial de órdenes por cliente.

**Gestión de Órdenes de Trabajo:**
- Crear órdenes asociadas a clientes con datos del vehículo.
- Asignar productos a órdenes con descuento automático de stock.
- Cambiar estado de órdenes.
- Listar órdenes con filtros por estado, cliente o fechas.

**Gestión de Inventario:**
- Registrar automáticamente movimientos de inventario.
- Consultar historial de movimientos por producto.
- Prevenir operaciones que resulten en stock negativo.

**Autenticación:**
- Validar credenciales y generar tokens JWT.
- Proteger endpoints con middleware de autenticación.

### Requisitos No Funcionales:

**Rendimiento:**
- Responder peticiones en menos de 2 segundos.
- Soportar al menos 100 productos y 500 órdenes.

**Seguridad:**
- Encriptación de contraseñas con bcrypt.
- Tokens JWT con expiración.
- Validación de datos en el backend.

**Mantenibilidad:**
- Código documentado y estructurado.
- Uso de Docker para despliegue.
- README con instrucciones claras.

## 9. Definición del MVP

### Funcionalidades Incluidas:
- Autenticación JWT.
- API REST con endpoints para:
  - CRUD completo de productos con búsqueda.
  - CRUD completo de clientes con búsqueda.
  - Crear, listar y cambiar estado de órdenes.
  - Asignar productos a órdenes con descuento de stock.
  - Consultar historial de órdenes y movimientos.
- MongoDB para persistencia.
- Redis para cache de productos.
- Proyecto dockerizado.
- Validaciones de reglas de negocio.

### Futuras Versiones:
- Interfaz gráfica web.
- Roles adicionales.
- Dashboard y reportes.
- Sistema de alertas.
- Aplicación móvil.

## 10. Lista Priorizada de Funcionalidades

### Prioridad Crítica (Imprescindible para MVP)
1. Autenticación JWT.
2. CRUD de productos con control de stock.
3. CRUD de clientes con búsqueda.
4. Crear y listar órdenes de trabajo.
5. Cambiar estado de órdenes.
6. Asignar productos a órdenes con descuento de stock.
7. Registro automático de movimientos de inventario.
8. Persistencia en MongoDB.
9. Cache de productos en Redis.
10. Proyecto dockerizado con documentación.

### Prioridad Alta (Post-MVP)
1. Filtros avanzados de órdenes.
2. Cálculo automático de precio total.
3. Sistema de alertas para stock bajo.
4. Reportes básicos.

### Prioridad Media
1. Interfaz gráfica web.
2. Dashboard con estadísticas.
3. Exportación de datos.

### Prioridad Baja
1. Roles adicionales.
2. Notificaciones automáticas.
3. Aplicación móvil.

## 11. Flujos Principales del Sistema

### 11.1. Autenticación
1. Usuario ingresa credenciales.
2. Sistema valida y genera token JWT.
3. Token se usa para acceder a endpoints protegidos.

### 11.2. Gestión de Productos
**Crear:** Completar datos → Validar → Guardar en BD → Registrar movimiento entrada → Actualizar cache.

**Editar:** Modificar datos → Validar → Si cambia stock registrar ajuste → Actualizar BD y cache.

**Eliminar:** Seleccionar producto → Verificar no esté en órdenes → Confirmar → Eliminar → Actualizar cache.

### 11.3. Gestión de Clientes
**Crear:** Completar datos → Validar formato → Guardar con fecha de registro.

**Eliminar:** Verificar no tenga órdenes → Confirmar → Eliminar.

### 11.4. Gestión de Órdenes
**Crear:** Seleccionar cliente → Ingresar datos vehículo y trabajo → Validar → Crear en estado Pendiente.

**Asignar Productos:** Buscar producto → Especificar cantidad → Verificar stock → Descontar stock → Registrar detalle → Actualizar precio total.

**Cambiar Estado:** Seleccionar orden → Cambiar a nuevo estado → Si es Completada registrar fecha finalización.

## 12. Modelo de Datos

### Relaciones:
- Cliente (1) → Órdenes de Trabajo (N)
- Orden de Trabajo (1) → Detalles de Orden (N)
- Detalle de Orden (N) → Producto (1)
- Producto (1) → Movimientos de Inventario (N)

## 13. Consideraciones Técnicas

### Stack Tecnológico:
- Backend: Node.js con Express
- Base de Datos: MongoDB
- Cache: Redis
- Autenticación: JWT con bcrypt
- Containerización: Docker y docker-compose

### Endpoints Principales:

**Autenticación:**
- POST /api/auth/login
- POST /api/auth/logout

**Productos:**
- GET /api/products (cache Redis)
- GET /api/products/:id
- POST /api/products
- PUT /api/products/:id
- DELETE /api/products/:id
- GET /api/products/search?q=

**Clientes:**
- GET /api/clients
- GET /api/clients/:id
- POST /api/clients
- PUT /api/clients/:id
- DELETE /api/clients/:id
- GET /api/clients/search?q=
- GET /api/clients/:id/orders

**Órdenes:**
- GET /api/orders
- GET /api/orders/:id
- POST /api/orders
- PUT /api/orders/:id/status
- POST /api/orders/:id/products
- GET /api/orders?status=&client=

**Inventario:**
- GET /api/inventory/movements/:productId

## 14. Cronograma Sugerido (4 semanas)

### Semana 1:
- Configuración inicial del proyecto.
- Docker y docker-compose.
- Conexión MongoDB y Redis.
- Autenticación JWT.
- CRUD de productos.

### Semana 2:
- CRUD de clientes.
- Búsquedas y filtros.
- Cache de productos en Redis.

### Semana 3:
- Gestión de órdenes de trabajo.
- Asignación de productos a órdenes.
- Movimientos de inventario.

### Semana 4:
- Validaciones completas.
- Pruebas de integración.
- Documentación.
- Ajustes finales.

## 15. Checklist del Proyecto

- [ ] Grupo formado (3-4 integrantes) y listado en README.
- [ ] REQUERIMIENTOS.md completo.
- [ ] Issues creados y asignados.
- [ ] Branching: main y develop.
- [ ] Al menos 5 commits descriptivos en develop.
- [ ] Docker y docker-compose funcional.
- [ ] README con instrucciones de ejecución.
- [ ] Autenticación JWT implementada.
- [ ] CRUD de productos completo.
- [ ] CRUD de clientes completo.
- [ ] Gestión de órdenes implementada.
- [ ] Asignación de productos a órdenes funcional.
- [ ] Movimientos de inventario automáticos.
- [ ] MongoDB configurado.
- [ ] Redis cache funcionando.
- [ ] Validaciones de reglas de negocio.
- [ ] API documentada (Postman/Swagger).
