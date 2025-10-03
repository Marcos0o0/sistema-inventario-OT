# REQUERIMIENTOS DEL SISTEMA

## 1. Descripción del Cliente y Problema Principal

El cliente es una pequeña empresa que maneja un negocio de reparaciones a vehículos y venta de repuestos automotrices. Actualmente, el dueño lleva un registro manual de inventarios y de las órdenes de trabajo que realiza. El problema principal es que este proceso manual es propenso a errores, consume mucho tiempo y no permite un control eficiente sobre el inventario ni las órdenes de trabajo.

La necesidad es crear un sistema digital que automatice estos registros y permita un control más preciso y rápido tanto del inventario como de las órdenes de trabajo.

## 2. Lista de Usuarios del Sistema

- Administrador

## 3. Usuarios y Permisos según Rol

**Administrador:**
- Crear, editar y eliminar productos.
- Gestionar el inventario y crear órdenes de trabajo.
- Ver historial de órdenes y reportes financieros.
- Gestionar clientes del sistema.
- Asignar productos a órdenes de trabajo.

## 4. Funciones Indispensables por Perfil

**Administrador (orden de prioridad):**
1. Autenticación y acceso seguro al sistema.
2. Gestión completa de productos.
3. Gestión de órdenes de trabajo.
4. Gestión de clientes.
5. Control de inventario y stock de productos.
6. Visualización de historial de órdenes.
7. Búsqueda y filtrado de productos, órdenes y clientes.

## 5. Datos Básicos a Almacenar

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

## 6. Reglas del Negocio

### 6.1. Usuarios y Roles
- Solo existe el rol de Administrador con acceso completo al sistema.
- El acceso requiere autenticación mediante credenciales válidas con JWT.

### 6.2. Productos e Inventario
- El precio de los productos debe ser mayor a cero.
- El stock no puede ser negativo en ninguna circunstancia.
- Al asignar productos a una orden, el stock se descuenta automáticamente.
- No se puede asignar más cantidad de producto que el stock disponible.
- Toda modificación de stock debe quedar registrada en Movimientos de Inventario.

### 6.3. Clientes
- Todas las órdenes de trabajo deben estar asociadas a un cliente registrado.
- Los datos obligatorios del cliente son: nombre completo y teléfono.
- No se puede eliminar un cliente que tenga órdenes de trabajo asociadas.

### 6.4. Órdenes de Trabajo
- Los estados posibles son: Pendiente, En progreso, Completada.
- Toda orden nueva se crea en estado Pendiente.
- Solo el Administrador puede cambiar el estado de una orden.
- No se pueden eliminar órdenes en estado En progreso o Completada.
- El precio total de la orden debe ser mayor a cero.
- La fecha de finalización no puede ser anterior a la fecha de emisión.
- Los datos del vehículo (marca, modelo, año, patente) son obligatorios.

### 6.5. Detalle de Órdenes
- Cada producto asignado a una orden debe registrarse en Detalle de Orden.
- Se debe guardar el precio unitario del producto al momento de asignarlo.
- La cantidad asignada debe ser mayor a cero.
- No se puede asignar un producto con stock insuficiente.

### 6.6. Movimientos de Inventario
- Todo cambio de stock debe registrarse con tipo, cantidad, fecha y descripción.
- Los tipos de movimiento válidos son: Entrada, Salida, Ajuste.
- No se permiten movimientos que generen stock negativo.

### 6.7. Seguridad y Persistencia
- El acceso se controla mediante autenticación JWT con tiempo de expiración.
- Las contraseñas deben almacenarse encriptadas.
- La información se guarda en MongoDB.
- Redis se utiliza para cachear la lista de productos.

### 6.8. Usabilidad
- La interfaz debe ser simple e intuitiva para usuarios no técnicos.
- Se debe solicitar confirmación antes de eliminar productos, órdenes o clientes.
- Las respuestas del sistema deben ser rápidas (menos de 2 segundos).

## 7. Requisitos Funcionales y No Funcionales

### Requisitos Funcionales:

**Gestión de Productos:**
- Registrar, editar, eliminar y listar productos.
- Buscar productos por nombre o categoría.
- Mostrar alertas cuando el stock sea bajo.

**Gestión de Clientes:**
- Registrar, editar, eliminar y listar clientes.
- Buscar clientes por nombre o teléfono.
- Mostrar historial de órdenes por cliente.

**Gestión de Órdenes de Trabajo:**
- Crear órdenes asociadas a clientes con datos del vehículo.
- Asignar productos a órdenes con descuento automático de stock.
- Cambiar estado de órdenes.
- Listar órdenes con filtros por estado, cliente o fechas.
- Calcular precio total de la orden basándose en productos asignados.

**Gestión de Inventario:**
- Registrar automáticamente movimientos de inventario.
- Consultar historial de movimientos por producto.
- Prevenir operaciones que resulten en stock negativo.

**Autenticación:**
- Validar credenciales y generar tokens JWT.
- Cerrar sesiones automáticamente después de inactividad.

### Requisitos No Funcionales:

**Usabilidad:**
- Interfaz intuitiva para personas no técnicas.
- Mensajes de error claros y orientadores.

**Rendimiento:**
- Cargar páginas en menos de 2 segundos.
- Búsquedas y filtros en menos de 1 segundo.
- Soportar al menos 100 productos y 500 órdenes sin degradación.

**Seguridad:**
- Encriptación de contraseñas.
- Protección contra inyecciones SQL y ataques XSS.
- Expiración automática de sesiones.

**Disponibilidad:**
- Funcionar en navegadores Chrome, Firefox y Edge actuales.

**Mantenibilidad:**
- Código documentado siguiendo buenas prácticas.
- Utilizar Docker para facilitar el despliegue.

## 8. Definición del MVP

### Funcionalidades Incluidas:
- Autenticación de usuarios mediante JWT.
- Rol de Administrador con acceso completo.
- API REST con endpoints para:
  - CRUD completo de productos con búsqueda.
  - CRUD completo de clientes con búsqueda.
  - Crear, listar y cambiar estado de órdenes de trabajo.
  - Asignar productos a órdenes con descuento automático de stock.
  - Consultar historial de órdenes.
- MongoDB para persistencia de datos.
- Redis para cachear GET /products.
- Proyecto dockerizado con docker-compose.
- README con instrucciones de ejecución.
- Validaciones de reglas de negocio.
- Registro automático de movimientos de inventario.

### Futuras Versiones:

**Versión 2.0:**
- Roles adicionales (Cliente, Vendedor).
- Interfaz gráfica web responsive.
- Dashboard con estadísticas y gráficos.
- Reportes automáticos.
- Sistema de alertas automáticas.

**Versión 3.0:**
- Notificaciones por correo o SMS.
- Aplicación móvil.
- Sistema de backup automático.
- Gestión de cotizaciones.
- Sistema de facturación.

## 9. Lista Priorizada de Funcionalidades

### Prioridad Crítica (Imprescindible para MVP)
1. Autenticación JWT para acceso seguro.
2. Gestión completa de productos (CRUD).
3. Control de stock con validación de negativos.
4. Gestión completa de clientes (CRUD y búsqueda).
5. Gestión de órdenes (crear, listar, cambiar estado).
6. Registro de datos del vehículo en órdenes.
7. Asignación de productos a órdenes con descuento de stock.
8. Registro de detalle de productos por orden.
9. Persistencia en MongoDB.
10. Cacheo de productos con Redis.
11. Proyecto dockerizado.

### Prioridad Alta (Después del MVP)
1. Búsqueda y filtros avanzados de órdenes.
2. Historial completo de movimientos de inventario.
3. Sistema de alertas para stock bajo.
4. Cálculo automático de precio total.
5. Reportes básicos de órdenes y productos.

### Prioridad Media (Mejoras a corto plazo)
1. Interfaz gráfica web básica.
2. Dashboard con estadísticas.
3. Gestión de proveedores.
4. Exportación de datos a Excel/PDF.
5. Sistema de respaldos periódicos.

### Prioridad Baja (Mejoras a largo plazo)
1. Reportes avanzados con gráficos.
2. Sistema de notificaciones automáticas.
3. Roles adicionales con permisos específicos.
4. Portal para clientes.
5. Aplicación móvil.

## 10. Flujos Principales del Sistema

### 10.1. Autenticación de Usuario
1. Usuario ingresa credenciales.
2. Sistema valida contra base de datos.
3. Si son correctas, genera token JWT y otorga acceso.
4. Si son incorrectas, muestra error y permanece en login.

### 10.2. Crear Producto
1. Administrador completa formulario con datos del producto.
2. Sistema valida campos obligatorios, precio mayor a cero y stock no negativo.
3. Sistema guarda producto en BD.
4. Sistema registra movimiento de inventario tipo "Entrada".
5. Sistema actualiza cache en Redis.
6. Muestra confirmación.

### 10.3. Editar Producto
1. Administrador selecciona producto y modifica datos.
2. Sistema valida nuevos datos.
3. Si cambió el stock, registra movimiento tipo "Ajuste".
4. Sistema actualiza producto en BD y cache.
5. Muestra confirmación.

### 10.4. Eliminar Producto
1. Administrador selecciona producto para eliminar.
2. Sistema verifica que no esté en órdenes existentes.
3. Sistema solicita confirmación.
4. Si confirma, elimina producto de BD y actualiza cache.
5. Si está en órdenes, muestra error y cancela operación.

### 10.5. Registrar Cliente
1. Administrador completa formulario con nombre, teléfono y correo opcional.
2. Sistema valida formatos.
3. Sistema guarda cliente con fecha de registro.
4. Muestra confirmación.

### 10.6. Crear Orden de Trabajo
1. Administrador selecciona o crea cliente.
2. Completa datos del vehículo y descripción del trabajo.
3. Ingresa fecha de entrega estimada.
4. Sistema valida datos obligatorios y fechas.
5. Sistema crea orden en estado "Pendiente" con fecha actual.
6. Muestra confirmación con ID de orden.

### 10.7. Asignar Productos a Orden
1. Administrador abre orden y busca producto.
2. Especifica cantidad a asignar.
3. Sistema verifica stock suficiente.
4. Si hay stock, registra detalle de orden con precio actual.
5. Sistema descuenta stock y registra movimiento "Salida".
6. Sistema recalcula precio total de orden.
7. Muestra confirmación o error si no hay stock.

### 10.8. Cambiar Estado de Orden
1. Administrador selecciona orden y nuevo estado.
2. Si marca "Completada", sistema registra fecha de finalización.
3. Sistema actualiza estado.
4. Muestra confirmación.

### 10.9. Consultar Historial
1. Administrador selecciona cliente u orden.
2. Sistema muestra todas las órdenes o movimientos relacionados ordenados por fecha.
3. Administrador puede ver detalle completo.

## 11. Modelo de Datos

### Relaciones:
- Un Cliente tiene múltiples Órdenes de Trabajo (1:N).
- Una Orden de Trabajo pertenece a un Cliente (N:1).
- Una Orden de Trabajo tiene múltiples Detalles de Orden (1:N).
- Un Detalle de Orden referencia a un Producto (N:1).
- Un Producto tiene múltiples Movimientos de Inventario (1:N).

## 12. Consideraciones Técnicas

### Stack Tecnológico:
- Backend: Node.js con Express
- Base de Datos: MongoDB
- Cache: Redis
- Autenticación: JWT
- Encriptación: bcrypt
- Containerización: Docker y docker-compose

### Arquitectura:
- Patrón: API REST
- Estructura: Controladores, Servicios, Modelos
- Validaciones: En capa de servicio
- Manejo de Errores: Middleware centralizado

### Endpoints Principales:

**Autenticación:**
- POST /api/auth/login
- POST /api/auth/logout

**Productos:**
- GET /api/products (con cache Redis)
- GET /api/products/:id
- POST /api/products
- PUT /api/products/:id
- DELETE /api/products/:id
- GET /api/products/search?q=nombre

**Clientes:**
- GET /api/clients
- GET /api/clients/:id
- POST /api/clients
- PUT /api/clients/:id
- DELETE /api/clients/:id
- GET /api/clients/search?q=nombre
- GET /api/clients/:id/orders

**Órdenes:**
- GET /api/orders
- GET /api/orders/:id
- POST /api/orders
- PUT /api/orders/:id/status
- POST /api/orders/:id/products
- GET /api/orders/filter?status=...&client=...

**Inventario:**
- GET /api/inventory/movements/:productId
- POST /api/inventory/adjust

## 13. Checklist del Proyecto

- [ ] Grupo formado (3-4 integrantes) y listado en README.
- [ ] REQUERIMIENTOS.md completo.
- [ ] Issues creados y asignados a integrantes.
- [ ] Branching: main y develop presentes.
- [ ] Al menos 5 commits descriptivos en develop.
- [ ] Proyecto dockerizado con docker-compose funcional.
- [ ] README con instrucciones de ejecución.
- [ ] API REST con todos los endpoints del MVP.
- [ ] Autenticación JWT implementada.
- [ ] Validaciones de reglas de negocio.
- [ ] MongoDB configurado y funcionando.
- [ ] Redis configurado para cache de productos.
- [ ] Pruebas básicas de endpoints.
