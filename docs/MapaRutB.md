## Stock Flow - API REST Backend Routes Map

### Información General
- **Sistema**: Stock Flow - Sistema de Gestión de Inventarios
- **Versión API**: v1.0
- **Base URL**: `https://api.stockflow.com/v1`
- **Autenticación**: JWT Bearer Token
- **Formato de respuesta**: JSON
- **Generado**: 2025-08-23 00:49:12 UTC
- **Documentado por**: ShivaAtom

---

## 🔐 Módulo de Autenticación y Seguridad
*Gestión de sesiones, login y tokens JWT*

| Método | Ruta | Descripción | Roles | Request Body | Response |
|--------|------|-------------|-------|--------------|----------|
| `POST` | `/auth/login` | Autenticación de usuario | `PUBLIC` | `{username, password}` | `{token, user, expiresIn}` |
| `POST` | `/auth/logout` | Cerrar sesión activa | `AUTHENTICATED` | `{}` | `{message}` |
| `POST` | `/auth/refresh` | Renovar token JWT | `AUTHENTICATED` | `{refreshToken}` | `{token, expiresIn}` |
| `GET` | `/auth/me` | Información del usuario actual | `AUTHENTICATED` | - | `{user, roles, permissions}` |
| `POST` | `/auth/change-password` | Cambiar contraseña | `AUTHENTICATED` | `{currentPassword, newPassword}` | `{message}` |
| `GET` | `/auth/sessions` | Listar sesiones activas | `AUTHENTICATED` | - | `{sessions[]}` |
| `DELETE` | `/auth/sessions/{sessionId}` | Invalidar sesión específica | `AUTHENTICATED` | - | `{message}` |
| `DELETE` | `/auth/sessions/all` | Invalidar todas las sesiones | `AUTHENTICATED` | - | `{message}` |

---

## 👥 Módulo de Gestión de Usuarios
*CRUD de empleados, roles y permisos (Solo ADMIN)*

| Método | Ruta | Descripción | Roles | Request Body | Response |
|--------|------|-------------|-------|--------------|----------|
| `GET` | `/users` | Listar todos los usuarios | `ADMIN` | - | `{users[], pagination}` |
| `GET` | `/users/{userId}` | Obtener usuario específico | `ADMIN` | - | `{user, roles}` |
| `POST` | `/users` | Crear nuevo usuario | `ADMIN` | `{username, email, password, firstName, lastName, phone}` | `{user, message}` |
| `PUT` | `/users/{userId}` | Actualizar usuario completo | `ADMIN` | `{firstName, lastName, phone, email}` | `{user, message}` |
| `PATCH` | `/users/{userId}/status` | Activar/desactivar usuario | `ADMIN` | `{isActive}` | `{user, message}` |
| `DELETE` | `/users/{userId}` | Eliminar usuario (soft delete) | `ADMIN` | - | `{message}` |
| `GET` | `/users/{userId}/roles` | Obtener roles del usuario | `ADMIN` | - | `{roles[]}` |
| `POST` | `/users/{userId}/roles` | Asignar rol al usuario | `ADMIN` | `{roleId}` | `{message}` |
| `DELETE` | `/users/{userId}/roles/{roleId}` | Remover rol del usuario | `ADMIN` | - | `{message}` |
| `GET` | `/users/search` | Buscar usuarios | `ADMIN` | `?q=term&status=active` | `{users[], pagination}` |

---

## 🏷️ Módulo de Roles y Permisos
*Gestión del sistema de roles (Solo ADMIN)*

| Método | Ruta | Descripción | Roles | Request Body | Response |
|--------|------|-------------|-------|--------------|----------|
| `GET` | `/roles` | Listar todos los roles | `ADMIN` | - | `{roles[]}` |
| `GET` | `/roles/{roleId}` | Obtener rol específico | `ADMIN` | - | `{role, permissions}` |
| `POST` | `/roles` | Crear nuevo rol | `ADMIN` | `{roleName, description}` | `{role, message}` |
| `PUT` | `/roles/{roleId}` | Actualizar rol | `ADMIN` | `{description}` | `{role, message}` |
| `DELETE` | `/roles/{roleId}` | Eliminar rol | `ADMIN` | - | `{message}` |
| `GET` | `/permissions` | Listar permisos disponibles | `ADMIN` | - | `{permissions[]}` |

---

## 📦 Módulo de Productos
*CRUD de productos del inventario*

| Método | Ruta | Descripción | Roles | Request Body | Response |
|--------|------|-------------|-------|--------------|----------|
| `GET` | `/products` | Listar todos los productos | `ADMIN, EMPLOYEE` | `?page=1&limit=20&category=&supplier=&status=` | `{products[], pagination}` |
| `GET` | `/products/{productId}` | Obtener producto específico | `ADMIN, EMPLOYEE` | - | `{product, stock, category, supplier}` |
| `POST` | `/products` | Crear nuevo producto | `ADMIN, EMPLOYEE` | `{productCode, productName, description, categoryId, supplierId, unitPrice, costPrice, unitOfMeasure, minStockLevel, maxStockLevel}` | `{product, message}` |
| `PUT` | `/products/{productId}` | Actualizar producto completo | `ADMIN, EMPLOYEE` | `{productName, description, categoryId, supplierId, unitPrice, costPrice, unitOfMeasure, minStockLevel, maxStockLevel}` | `{product, message}` |
| `PATCH` | `/products/{productId}/status` | Activar/desactivar producto | `ADMIN` | `{isActive}` | `{product, message}` |
| `PATCH` | `/products/{productId}/prices` | Actualizar precios | `ADMIN` | `{unitPrice, costPrice}` | `{product, message}` |
| `PATCH` | `/products/{productId}/stock-levels` | Actualizar niveles de stock | `ADMIN, EMPLOYEE` | `{minStockLevel, maxStockLevel}` | `{product, message}` |
| `DELETE` | `/products/{productId}` | Eliminar producto | `ADMIN` | - | `{message}` |
| `GET` | `/products/search` | Buscar productos | `ADMIN, EMPLOYEE` | `?q=term&category=&supplier=&lowStock=true` | `{products[], pagination}` |
| `GET` | `/products/low-stock` | Productos con stock bajo | `ADMIN, EMPLOYEE` | - | `{products[]}` |
| `GET` | `/products/code/{productCode}` | Buscar por código de producto | `ADMIN, EMPLOYEE` | - | `{product}` |

---

## 📁 Módulo de Categorías
*Gestión de categorías de productos*

| Método | Ruta | Descripción | Roles | Request Body | Response |
|--------|------|-------------|-------|--------------|----------|
| `GET` | `/categories` | Listar todas las categorías | `ADMIN, EMPLOYEE` | `?status=active` | `{categories[]}` |
| `GET` | `/categories/{categoryId}` | Obtener categoría específica | `ADMIN, EMPLOYEE` | - | `{category, productsCount}` |
| `POST` | `/categories` | Crear nueva categoría | `ADMIN` | `{categoryName, description}` | `{category, message}` |
| `PUT` | `/categories/{categoryId}` | Actualizar categoría | `ADMIN` | `{categoryName, description}` | `{category, message}` |
| `PATCH` | `/categories/{categoryId}/status` | Activar/desactivar categoría | `ADMIN` | `{isActive}` | `{category, message}` |
| `DELETE` | `/categories/{categoryId}` | Eliminar categoría | `ADMIN` | - | `{message}` |
| `GET` | `/categories/{categoryId}/products` | Productos de una categoría | `ADMIN, EMPLOYEE` | `?status=active` | `{products[]}` |

---

## 🏢 Módulo de Proveedores
*Gestión de proveedores de productos*

| Método | Ruta | Descripción | Roles | Request Body | Response |
|--------|------|-------------|-------|--------------|----------|
| `GET` | `/suppliers` | Listar todos los proveedores | `ADMIN, EMPLOYEE` | `?status=active` | `{suppliers[]}` |
| `GET` | `/suppliers/{supplierId}` | Obtener proveedor específico | `ADMIN, EMPLOYEE` | - | `{supplier, productsCount}` |
| `POST` | `/suppliers` | Crear nuevo proveedor | `ADMIN` | `{supplierName, contactPerson, email, phone, address, taxId}` | `{supplier, message}` |
| `PUT` | `/suppliers/{supplierId}` | Actualizar proveedor | `ADMIN` | `{supplierName, contactPerson, email, phone, address}` | `{supplier, message}` |
| `PATCH` | `/suppliers/{supplierId}/status` | Activar/desactivar proveedor | `ADMIN` | `{isActive}` | `{supplier, message}` |
| `DELETE` | `/suppliers/{supplierId}` | Eliminar proveedor | `ADMIN` | - | `{message}` |
| `GET` | `/suppliers/{supplierId}/products` | Productos de un proveedor | `ADMIN, EMPLOYEE` | `?status=active` | `{products[]}` |
| `GET` | `/suppliers/search` | Buscar proveedores | `ADMIN, EMPLOYEE` | `?q=term&status=active` | `{suppliers[]}` |

---

## 📊 Módulo de Inventario y Stock
*Gestión de stock y movimientos de inventario*

| Método | Ruta | Descripción | Roles | Request Body | Response |
|--------|------|-------------|-------|--------------|----------|
| `GET` | `/inventory/stock` | Stock actual de todos los productos | `ADMIN, EMPLOYEE` | `?lowStock=true&category=&supplier=` | `{stock[], pagination}` |
| `GET` | `/inventory/stock/{productId}` | Stock de producto específico | `ADMIN, EMPLOYEE` | - | `{stock, product}` |
| `POST` | `/inventory/movements` | Registrar movimiento de inventario | `ADMIN, EMPLOYEE` | `{productId, movementTypeId, quantity, unitPrice, referenceNumber, notes, movementDate}` | `{movement, newStock, message}` |
| `GET` | `/inventory/movements` | Historial de movimientos | `ADMIN, EMPLOYEE` | `?page=1&limit=20&productId=&type=&dateFrom=&dateTo=&userId=` | `{movements[], pagination}` |
| `GET` | `/inventory/movements/{movementId}` | Obtener movimiento específico | `ADMIN, EMPLOYEE` | - | `{movement, product, user}` |
| `PATCH` | `/inventory/stock/{productId}/adjust` | Ajuste de stock (inventario físico) | `ADMIN, EMPLOYEE` | `{newQuantity, reason, notes}` | `{stock, movement, message}` |
| `POST` | `/inventory/stock/{productId}/reserve` | Reservar stock | `ADMIN, EMPLOYEE` | `{quantity, reason}` | `{stock, message}` |
| `POST` | `/inventory/stock/{productId}/release` | Liberar stock reservado | `ADMIN, EMPLOYEE` | `{quantity}` | `{stock, message}` |
| `GET` | `/inventory/movement-types` | Tipos de movimientos disponibles | `ADMIN, EMPLOYEE` | - | `{movementTypes[]}` |
| `GET` | `/inventory/valuation` | Valoración total del inventario | `ADMIN` | `?date=2025-08-23&category=` | `{totalValue, productCount, details[]}` |

---

## 📈 Módulo de Reportes
*Generación y gestión de reportes del sistema*

| Método | Ruta | Descripción | Roles | Request Body | Response |
|--------|------|-------------|-------|--------------|----------|
| `GET` | `/reports` | Listar reportes generados | `ADMIN, EMPLOYEE` | `?page=1&limit=20&type=&dateFrom=&dateTo=` | `{reports[], pagination}` |
| `GET` | `/reports/{reportId}` | Obtener reporte específico | `ADMIN, EMPLOYEE` | - | `{report, downloadUrl}` |
| `POST` | `/reports/generate` | Generar nuevo reporte | `ADMIN, EMPLOYEE` | `{reportType, parameters, format, name}` | `{report, jobId, message}` |
| `GET` | `/reports/{reportId}/download` | Descargar archivo de reporte | `ADMIN, EMPLOYEE` | - | `File Stream` |
| `DELETE` | `/reports/{reportId}` | Eliminar reporte | `ADMIN, EMPLOYEE` | - | `{message}` |
| `GET` | `/reports/types` | Tipos de reportes disponibles | `ADMIN, EMPLOYEE` | - | `{reportTypes[]}` |
| `GET` | `/reports/job/{jobId}/status` | Estado de generación de reporte | `ADMIN, EMPLOYEE` | - | `{status, progress, result}` |

### Tipos de Reportes Disponibles:
- `STOCK_ACTUAL`: Stock actual de todos los productos
- `MOVIMIENTOS_PERIODO`: Movimientos en rango de fechas
- `PRODUCTOS_BAJO_STOCK`: Productos por debajo del stock mínimo
- `VALORACION_INVENTARIO`: Valoración monetaria del inventario
- `ACTIVIDAD_USUARIOS`: Actividad de usuarios en el sistema

---

## 📊 Módulo de Dashboard y Métricas
*Información estadística y métricas del sistema*

| Método | Ruta | Descripción | Roles | Request Body | Response |
|--------|------|-------------|-------|--------------|----------|
| `GET` | `/dashboard/metrics` | Métricas generales del sistema | `ADMIN, EMPLOYEE` | `?period=7d` | `{totalProducts, totalValue, lowStockCount, movementsToday, alerts[]}` |
| `GET` | `/dashboard/stock-alerts` | Alertas de stock bajo/alto | `ADMIN, EMPLOYEE` | - | `{lowStockProducts[], overStockProducts[]}` |
| `GET` | `/dashboard/recent-movements` | Movimientos recientes | `ADMIN, EMPLOYEE` | `?limit=10` | `{movements[]}` |
| `GET` | `/dashboard/top-products` | Productos con más movimiento | `ADMIN, EMPLOYEE` | `?period=30d&limit=10` | `{products[]}` |
| `GET` | `/dashboard/category-distribution` | Distribución por categorías | `ADMIN, EMPLOYEE` | - | `{categories[], chartData}` |
| `GET` | `/dashboard/movements-chart` | Datos para gráfico de movimientos | `ADMIN, EMPLOYEE` | `?period=30d&type=daily` | `{chartData[], labels[]}` |

---

## 🔍 Módulo de Auditoría
*Logs y trazabilidad del sistema (Solo ADMIN)*

| Método | Ruta | Descripción | Roles | Request Body | Response |
|--------|------|-------------|-------|--------------|----------|
| `GET` | `/audit/logs` | Logs de auditoría | `ADMIN` | `?page=1&limit=50&userId=&action=&table=&dateFrom=&dateTo=` | `{logs[], pagination}` |
| `GET` | `/audit/logs/{logId}` | Detalle de log específico | `ADMIN` | - | `{log, changes, user}` |
| `GET` | `/audit/user-activity/{userId}` | Actividad de usuario específico | `ADMIN` | `?dateFrom=&dateTo=` | `{activities[], summary}` |
| `GET` | `/audit/system-health` | Estado general del sistema | `ADMIN` | - | `{uptime, dbHealth, apiHealth, errors[]}` |

---

## 🔧 Módulo de Configuración del Sistema
*Configuraciones generales (Solo ADMIN)*

| Método | Ruta | Descripción | Roles | Request Body | Response |
|--------|------|-------------|-------|--------------|----------|
| `GET` | `/config/settings` | Configuraciones del sistema | `ADMIN` | - | `{settings{}}` |
| `PUT` | `/config/settings` | Actualizar configuraciones | `ADMIN` | `{key: value}` | `{settings, message}` |
| `POST` | `/config/backup` | Crear backup del sistema | `ADMIN` | `{includeLogs}` | `{backupId, downloadUrl}` |
| `GET` | `/config/version` | Información de versión | `PUBLIC` | - | `{version, buildDate, environment}` |

---

## 📋 Convenciones y Estándares de la API

### Códigos de Respuesta HTTP:
- `200 OK`: Operación exitosa
- `201 Created`: Recurso creado exitosamente
- `400 Bad Request`: Error en parámetros de entrada
- `401 Unauthorized`: Token JWT inválido o expirado
- `403 Forbidden`: Sin permisos para la operación
- `404 Not Found`: Recurso no encontrado
- `409 Conflict`: Conflicto de datos (ej: código duplicado)
- `422 Unprocessable Entity`: Error de validación
- `500 Internal Server Error`: Error interno del servidor

### Estructura de Respuesta Estándar:
```json
{
  "success": true/false,
  "message": "Mensaje descriptivo",
  "data": {}, // Datos solicitados
  "errors": [], // Array de errores si aplica
  "pagination": { // Solo en listas
    "page": 1,
    "limit": 20,
    "total": 150,
    "totalPages": 8
  },
  "timestamp": "2025-08-23T00:49:12Z"
}
```

### Headers Requeridos:
- `Authorization: Bearer {jwt_token}` (excepto rutas públicas)
- `Content-Type: application/json`
- `Accept: application/json`

### Parámetros de Paginación Estándar:
- `page`: Número de página (default: 1)
- `limit`: Elementos por página (default: 20, max: 100)
- `sort`: Campo de ordenamiento (default: 'id')
- `order`: Dirección del ordenamiento ('asc'/'desc', default: 'desc')

---