# REQUERIMIENTOS

## 1. Descripción del Cliente y Problema Principal

El cliente es una pequeña empresa que maneja un negocio de reparaciones a vehiculos y venta de repuestos automotrices. Actualmente, el dueño lleva un registro manual de inventarios y de las órdenes de trabajo que realiza. El problema principal es que este proceso manual es propenso a errores, consume mucho tiempo y no permite un control eficiente sobre el inventario ni las órdenes de trabajo, lo que podría llevar a pérdidas de productos o a desorganización en el flujo de trabajo.

La necesidad es crear un sistema digital que automatice estos registros y permita un control más preciso y rápido tanto del inventario como de las órdenes de trabajo.

## 2. Lista de Usuarios del Sistema

- Administrador

## 3. Usuarios y Permisos según rol

Principal y único rol del sistema y que permisos tiene:

- **Administrador**:
  - Crear, e ditar y eliminar productos.
  - Ver reportes financieros completos.
  - Gestionar el inventario y crear ordenes de trabajo.
  - Ver historial de ordenes

## 4. Funciones Indispensables por Perfil (Lista Priorizada)

Lista las funciones más importantes para cada perfil. Ordena las funciones de mayor a menor importancia.

### Ejemplo:
- **Administrador**:
  - Acceso completo a la gestión de productos.
  - Gestión de usuarios.
  
- **Usuario/Cliente**:
  - Búsqueda de productos por categorías.
  - Realizar compras.

## 5. Datos Básicos a Almacenar (Entidades y Atributos)

Producto:
 - ID
 - Nombre
 - Precio
 - Stock
 - Categoría
 - Descripción
 - Proveedor
 - Fecha ingreso

Orden de trabajo:
 - ID
 - Fecha de emisión
 - Cliente
 - Descripción del trabajo
 - Estado
 - Fecha de entrega estimada
 - Fecha de finalización
 - Precio

Cliente:
 - ID
 - Nombre completo
 - Teléfono
 - Correo

Movimientos Inventario:
 - ID
 - Producto
 - Cantidad
 - Tipo de movimiento
 - Fecha de movimiento
 - Descripción

## 6. Reglas del Negocio importantes

### 1. Usuarios y Roles
- Solo existe el rol **Administrador** con acceso completo.
- Solo el Administrador puede gestionar productos, órdenes y clientes.

### 2. Productos e Inventario
- Los productos deben contener: ID, nombre, precio, stock, categoría, proveedor y fecha de ingreso.
- El stock no puede ser negativo.
- Al asignar productos a una orden, el stock se descuenta automáticamente.
- No se puede asignar más producto que el stock disponible.

### 3. Órdenes de Trabajo
- Cada orden tiene ID, cliente, descripción, fechas, estado y precio.
- Estados posibles: Pendiente, En progreso, Completada.
- Solo el Administrador puede cambiar el estado.
- No se pueden eliminar órdenes en progreso o completadas.

### 4. Clientes
- Las órdenes deben estar asociadas a clientes registrados.
- El Administrador puede crear, editar y eliminar clientes.

### 5. Movimientos de Inventario
- Todo cambio de stock debe registrarse con tipo, cantidad, fecha y descripción.
- No se permiten movimientos que generen stock negativo.

## 6. Seguridad y Persistencia
- El acceso se controla mediante autenticación JWT.
- La información se guarda en MongoDB.
- Redis se usa para cachear la lista de productos.

### 7. Usabilidad y Restricciones
- La interfaz debe ser simple e intuitiva para usuarios no técnicos.
- Se debe pedir confirmación para eliminar productos u órdenes.
- El sistema debe estar disponible en PCs modernas.
- Las respuestas deben ser rápidas (menos de 2 segundos).


## 7. Lista de Requisitos Funcionales y No Funcionales

### Requisitos Funcionales:
- El sistema debe permitir registrar productos en el inventario.
- El sistema debe permitir ver la lista de productos y sus cantidades.
- El sistema debe permitir registrar nuevas órdenes de trabajo.
- El sistema debe permitir cambiar el estado de una orden (pendiente, en progreso, completada).
- El sistema debe permitir ver el historial de órdenes de trabajo.
- El sistema debe permitir asignar productos a una orden de trabajo.
- El sistema debe permitir editar o eliminar productos y órdenes.

### Requisitos No Funcionales:
- El sistema debe ser fácil de usar para personas no técnicas (interfaz intuitiva).
- El sistema debe guardar la información de forma segura (persistencia de datos).
- El sistema debe estar disponible desde cualquier computador moderno.
- El sistema debe cargar las páginas en menos de 2 segundos.
- El sistema debe permitir hacer respaldos/exportar datos (en versiones futuras).

## 8. Definición del MVP

### MVP (Minimum Viable Product):

- Autenticación de usuarios mediante JWT.
- Rol de administrador con acceso completo a productos y órdenes.
- API REST con endpoints para:
  - Crear, editar, eliminar y listar productos.
  - Crear y listar órdenes de trabajo.
  - Cambiar estado de órdenes.
- MongoDB como base de datos para persistencia de información.
- Redis para cachear la ruta `GET /products`.
- Proyecto usando Docker y docker-compose.
- README con instrucciones para ejecución con `docker compose`.

### Futuras versiones:

- Implementación de roles adicionales: Cliente y Vendedor.
- Registro y visualización de clientes.
- Asignación de productos a órdenes (actualmente básico).
- Historial de movimientos del inventario.
- Reportes automáticos (ventas, productos más usados, etc.).
- Sistema de notificaciones o alertas (por ejemplo, stock bajo).
- Interfaz gráfica web o móvil.
- Sistema de backup/exportación de datos.

## 9. Lista Priorizada de Funcionalidades

### Prioridad Alta (Imprescindible para el MVP)
1. Autenticación JWT para acceso seguro.
2. Gestión completa de productos (crear, editar, eliminar, listar y stock).
3. Gestión de órdenes (crear, listar y cambiar estado).
4. Asignar productos a órdenes y descontar stock.
5. Persistencia en MongoDB.
6. Cacheo de productos con Redis.
7. Proyecto dockerizado para fácil despliegue.

---

### Prioridad Media (Para después del MVP)
1. Historial y filtros de órdenes de trabajo.
2. Registro de movimientos de inventario.
3. Gestión básica de clientes y relación con órdenes.
4. Interfaz gráfica mínima (web o móvil).

---

### Prioridad Baja (Futuras mejoras)
1. Reportes automáticos (ventas, productos más usados, etc.).
2. Alertas y notificaciones (stock bajo, órdenes vencidas).
3. Exportación de datos y backups.
4. Roles adicionales (Vendedor, Cliente).
5. Logs y auditoría de acciones.
6. Soporte multidioma y personalización de interfaz.

## 10. Checklist Mínimo

- [✔] Grupo formado (3-4 integrantes) y listado en README del repo.
- [✔] REQUERIMIENTOS.md completo.
- [ ] Issues creados y asignados a integrantes.
- [✔] Branching mínimo: main y develop presentes.
- [ ] Al menos 5 commits descriptivos en develop.
