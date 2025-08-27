## Stock Flow - Frontend Routes Map (SPA)

### Información General
- **Sistema**: Stock Flow - Sistema de Gestión de Inventarios
- **Tipo**: Single Page Application (SPA)
- **Framework sugerido**: React.js / Vue.js / Angular
- **Router**: React Router / Vue Router / Angular Router
- **Base URL**: `https://app.stockflow.com`
- **Autenticación**: JWT Token + Guards
- **Generado**: 2025-08-23 00:52:12 UTC
- **Documentado por**: ShivaAtom

---

## 🔐 Módulo de Autenticación
*Rutas públicas para login y gestión de sesión*

| Ruta | Componente | Descripción | Guard | Props/Estado | Redirección |
|------|------------|-------------|--------|--------------|-------------|
| `/` | `HomePage` | Página de inicio (redirect) | `PublicOnly` | - | → `/login` o `/dashboard` |
| `/login` | `LoginPage` | Formulario de autenticación | `PublicOnly` | `{returnUrl}` | → `/dashboard` |
| `/forgot-password` | `ForgotPasswordPage` | Recuperar contraseña | `PublicOnly` | - | → `/login` |
| `/reset-password/:token` | `ResetPasswordPage` | Restablecer contraseña | `PublicOnly` | `{token}` | → `/login` |
| `/unauthorized` | `UnauthorizedPage` | Acceso denegado (403) | `Public` | `{message}` | - |
| `/not-found` | `NotFoundPage` | Página no encontrada (404) | `Public` | - | - |

### Componentes de Autenticación:
```
src/
├── components/auth/
│   ├── LoginForm.vue           // Formulario de login con validación
│   ├── LogoutButton.vue        // Botón de cerrar sesión
│   ├── SessionTimeout.vue      // Modal de expiración de sesión
│   └── AuthGuard.vue          // Guard de autenticación
├── pages/auth/
│   ├── LoginPage.vue          // Página completa de login
│   ├── ForgotPasswordPage.vue // Página de recuperación
│   └── ResetPasswordPage.vue  // Página de restablecimiento
```

---

## 🏠 Módulo de Dashboard Principal
*Panel principal con métricas y resumen del sistema*

| Ruta | Componente | Descripción | Roles | Props/Estado | Datos API |
|------|------------|-------------|--------|--------------|-----------|
| `/dashboard` | `DashboardPage` | Panel principal del sistema | `ADMIN, EMPLOYEE` | `{period: '7d'}` | `/dashboard/metrics` |
| `/dashboard/alerts` | `AlertsPage` | Centro de alertas y notificaciones | `ADMIN, EMPLOYEE` | - | `/dashboard/stock-alerts` |

### Componentes del Dashboard:
```
src/
├── components/dashboard/
│   ├── MetricsCard.vue         // Tarjetas de métricas (stock, ventas, etc.)
│   ├── StockAlertsWidget.vue   // Widget de alertas de stock
│   ├── RecentMovementsTable.vue // Tabla de movimientos recientes
│   ├── CategoryChart.vue       // Gráfico de distribución por categorías
│   ├── MovementsChart.vue      // Gráfico de movimientos en el tiempo
│   └── TopProductsList.vue     // Lista de productos más movidos
├── pages/dashboard/
│   ├── DashboardPage.vue       // Dashboard principal
│   └── AlertsPage.vue          // Página de alertas
```

---

## 📦 Módulo de Gestión de Productos
*CRUD completo de productos del inventario*

| Ruta | Componente | Descripción | Roles | Props/Estado | Datos API |
|------|------------|-------------|--------|--------------|-----------|
| `/products` | `ProductsListPage` | Lista de todos los productos | `ADMIN, EMPLOYEE` | `{page, filters}` | `/products` |
| `/products/new` | `ProductCreatePage` | Crear nuevo producto | `ADMIN, EMPLOYEE` | - | `/categories`, `/suppliers` |
| `/products/:id` | `ProductDetailPage` | Detalle de producto específico | `ADMIN, EMPLOYEE` | `{productId}` | `/products/:id` |
| `/products/:id/edit` | `ProductEditPage` | Editar producto existente | `ADMIN, EMPLOYEE` | `{productId}` | `/products/:id` |
| `/products/:id/stock` | `ProductStockPage` | Gestión de stock del producto | `ADMIN, EMPLOYEE` | `{productId}` | `/inventory/stock/:id` |
| `/products/:id/movements` | `ProductMovementsPage` | Historial de movimientos | `ADMIN, EMPLOYEE` | `{productId}` | `/inventory/movements` |
| `/products/low-stock` | `LowStockPage` | Productos con stock bajo | `ADMIN, EMPLOYEE` | - | `/products/low-stock` |
| `/products/search` | `ProductSearchPage` | Búsqueda avanzada | `ADMIN, EMPLOYEE` | `{query}` | `/products/search` |

### Componentes de Productos:
```
src/
├── components/products/
│   ├── ProductCard.vue         // Tarjeta de producto en lista
│   ├── ProductForm.vue         // Formulario crear/editar producto
│   ├── ProductFilters.vue      // Filtros de búsqueda
│   ├── ProductTable.vue        // Tabla de productos
│   ├── StockAdjustModal.vue    // Modal para ajuste de stock
│   ├── ProductQRCode.vue       // Generador de código QR
│   └── BulkActionsBar.vue      // Barra de acciones masivas
├── pages/products/
│   ├── ProductsListPage.vue    // Lista principal de productos
│   ├── ProductCreatePage.vue   // Crear producto
│   ├── ProductDetailPage.vue   // Detalle del producto
│   ├── ProductEditPage.vue     // Editar producto
│   ├── ProductStockPage.vue    // Gestión de stock
│   ├── ProductMovementsPage.vue // Historial de movimientos
│   ├── LowStockPage.vue        // Productos con stock bajo
│   └── ProductSearchPage.vue   // Búsqueda avanzada
```

---

## 📊 Módulo de Inventario y Stock
*Gestión de stock y movimientos de inventario*

| Ruta | Componente | Descripción | Roles | Props/Estado | Datos API |
|------|------------|-------------|--------|--------------|-----------|
| `/inventory` | `InventoryOverviewPage` | Resumen general del inventario | `ADMIN, EMPLOYEE` | - | `/inventory/stock` |
| `/inventory/stock` | `StockListPage` | Stock actual de todos los productos | `ADMIN, EMPLOYEE` | `{filters}` | `/inventory/stock` |
| `/inventory/movements` | `MovementsListPage` | Historial de movimientos | `ADMIN, EMPLOYEE` | `{filters, page}` | `/inventory/movements` |
| `/inventory/movements/new` | `NewMovementPage` | Registrar nuevo movimiento | `ADMIN, EMPLOYEE` | `{productId?}` | `/products`, `/inventory/movement-types` |
| `/inventory/movements/:id` | `MovementDetailPage` | Detalle de movimiento | `ADMIN, EMPLOYEE` | `{movementId}` | `/inventory/movements/:id` |
| `/inventory/adjust` | `StockAdjustmentPage` | Ajuste masivo de inventario | `ADMIN, EMPLOYEE` | - | `/inventory/stock` |
| `/inventory/valuation` | `InventoryValuationPage` | Valoración del inventario | `ADMIN` | `{date}` | `/inventory/valuation` |

### Componentes de Inventario:
```
src/
├── components/inventory/
│   ├── StockTable.vue          // Tabla de stock actual
│   ├── MovementForm.vue        // Formulario de movimiento
│   ├── MovementsTable.vue      // Tabla de movimientos
│   ├── StockFilters.vue        // Filtros de stock
│   ├── QuickMovementModal.vue  // Modal movimiento rápido
│   ├── BarcodeScanner.vue      // Escáner de códigos de barras
│   └── InventoryChart.vue      // Gráficos de inventario
├── pages/inventory/
│   ├── InventoryOverviewPage.vue // Resumen del inventario
│   ├── StockListPage.vue       // Lista de stock
│   ├── MovementsListPage.vue   // Lista de movimientos
│   ├── NewMovementPage.vue     // Nuevo movimiento
│   ├── MovementDetailPage.vue  // Detalle de movimiento
│   ├── StockAdjustmentPage.vue // Ajuste de stock
│   └── InventoryValuationPage.vue // Valoración
```

---

## 🏷️ Módulo de Categorías
*Gestión de categorías de productos*

| Ruta | Componente | Descripción | Roles | Props/Estado | Datos API |
|------|------------|-------------|--------|--------------|-----------|
| `/categories` | `CategoriesListPage` | Lista de todas las categorías | `ADMIN, EMPLOYEE` | - | `/categories` |
| `/categories/new` | `CategoryCreatePage` | Crear nueva categoría | `ADMIN` | - | - |
| `/categories/:id` | `CategoryDetailPage` | Detalle de categoría | `ADMIN, EMPLOYEE` | `{categoryId}` | `/categories/:id` |
| `/categories/:id/edit` | `CategoryEditPage` | Editar categoría | `ADMIN` | `{categoryId}` | `/categories/:id` |
| `/categories/:id/products` | `CategoryProductsPage` | Productos de la categoría | `ADMIN, EMPLOYEE` | `{categoryId}` | `/categories/:id/products` |

### Componentes de Categorías:
```
src/
├── components/categories/
│   ├── CategoryCard.vue        // Tarjeta de categoría
│   ├── CategoryForm.vue        // Formulario de categoría
│   ├── CategoryTree.vue        // Árbol de categorías
│   └── CategorySelector.vue    // Selector de categoría
├── pages/categories/
│   ├── CategoriesListPage.vue  // Lista de categorías
│   ├── CategoryCreatePage.vue  // Crear categoría
│   ├── CategoryDetailPage.vue  // Detalle de categoría
│   ├── CategoryEditPage.vue    // Editar categoría
│   └── CategoryProductsPage.vue // Productos de categoría
```

---

## 🏢 Módulo de Proveedores
*Gestión de proveedores de productos*

| Ruta | Componente | Descripción | Roles | Props/Estado | Datos API |
|------|------------|-------------|--------|--------------|-----------|
| `/suppliers` | `SuppliersListPage` | Lista de todos los proveedores | `ADMIN, EMPLOYEE` | - | `/suppliers` |
| `/suppliers/new` | `SupplierCreatePage` | Crear nuevo proveedor | `ADMIN` | - | - |
| `/suppliers/:id` | `SupplierDetailPage` | Detalle de proveedor | `ADMIN, EMPLOYEE` | `{supplierId}` | `/suppliers/:id` |
| `/suppliers/:id/edit` | `SupplierEditPage` | Editar proveedor | `ADMIN` | `{supplierId}` | `/suppliers/:id` |
| `/suppliers/:id/products` | `SupplierProductsPage` | Productos del proveedor | `ADMIN, EMPLOYEE` | `{supplierId}` | `/suppliers/:id/products` |

### Componentes de Proveedores:
```
src/
├── components/suppliers/
│   ├── SupplierCard.vue        // Tarjeta de proveedor
│   ├── SupplierForm.vue        // Formulario de proveedor
│   ├── SupplierContactInfo.vue // Información de contacto
│   └── SupplierSelector.vue    // Selector de proveedor
├── pages/suppliers/
│   ├── SuppliersListPage.vue   // Lista de proveedores
│   ├── SupplierCreatePage.vue  // Crear proveedor
│   ├── SupplierDetailPage.vue  // Detalle de proveedor
│   ├── SupplierEditPage.vue    // Editar proveedor
│   └── SupplierProductsPage.vue // Productos del proveedor
```

---

## 📈 Módulo de Reportes
*Generación y gestión de reportes del sistema*

| Ruta | Componente | Descripción | Roles | Props/Estado | Datos API |
|------|------------|-------------|--------|--------------|-----------|
| `/reports` | `ReportsListPage` | Lista de reportes generados | `ADMIN, EMPLOYEE` | `{filters}` | `/reports` |
| `/reports/new` | `ReportGeneratorPage` | Generador de reportes | `ADMIN, EMPLOYEE` | - | `/reports/types` |
| `/reports/:id` | `ReportDetailPage` | Detalle y descarga de reporte | `ADMIN, EMPLOYEE` | `{reportId}` | `/reports/:id` |
| `/reports/templates` | `ReportTemplatesPage` | Plantillas de reportes | `ADMIN` | - | `/reports/types` |
| `/reports/scheduled` | `ScheduledReportsPage` | Reportes programados | `ADMIN` | - | `/reports/scheduled` |

### Componentes de Reportes:
```
src/
├── components/reports/
│   ├── ReportCard.vue          // Tarjeta de reporte
│   ├── ReportGenerator.vue     // Generador de reportes
│   ├── ReportFilters.vue       // Filtros de reportes
│   ├── ReportPreview.vue       // Vista previa de reporte
│   ├── ReportScheduler.vue     // Programador de reportes
│   └── DownloadButton.vue      // Botón de descarga
├── pages/reports/
│   ├── ReportsListPage.vue     // Lista de reportes
│   ├── ReportGeneratorPage.vue // Generador de reportes
│   ├── ReportDetailPage.vue    // Detalle de reporte
│   ├── ReportTemplatesPage.vue // Plantillas de reportes
│   └── ScheduledReportsPage.vue // Reportes programados
```

---

## 👥 Módulo de Administración de Usuarios
*Gestión de usuarios y roles (Solo ADMIN)*

| Ruta | Componente | Descripción | Roles | Props/Estado | Datos API |
|------|------------|-------------|--------|--------------|-----------|
| `/admin` | `AdminDashboardPage` | Panel de administración | `ADMIN` | - | `/dashboard/metrics` |
| `/admin/users` | `UsersListPage` | Lista de todos los usuarios | `ADMIN` | `{filters}` | `/users` |
| `/admin/users/new` | `UserCreatePage` | Crear nuevo usuario | `ADMIN` | - | `/roles` |
| `/admin/users/:id` | `UserDetailPage` | Detalle de usuario | `ADMIN` | `{userId}` | `/users/:id` |
| `/admin/users/:id/edit` | `UserEditPage` | Editar usuario | `ADMIN` | `{userId}` | `/users/:id` |
| `/admin/roles` | `RolesManagementPage` | Gestión de roles y permisos | `ADMIN` | - | `/roles` |
| `/admin/audit` | `AuditLogsPage` | Logs de auditoría | `ADMIN` | `{filters}` | `/audit/logs` |
| `/admin/settings` | `SystemSettingsPage` | Configuración del sistema | `ADMIN` | - | `/config/settings` |

### Componentes de Administración:
```
src/
├── components/admin/
│   ├── UserCard.vue            // Tarjeta de usuario
│   ├── UserForm.vue            // Formulario de usuario
│   ├── RoleSelector.vue        // Selector de roles
│   ├── PermissionsMatrix.vue   // Matriz de permisos
│   ├── AuditLogTable.vue       // Tabla de logs de auditoría
│   ├── SystemHealthWidget.vue  // Widget de salud del sistema
│   └── BackupManager.vue       // Gestor de backups
├── pages/admin/
│   ├── AdminDashboardPage.vue  // Dashboard de admin
│   ├── UsersListPage.vue       // Lista de usuarios
│   ├── UserCreatePage.vue      // Crear usuario
│   ├── UserDetailPage.vue      // Detalle de usuario
│   ├── UserEditPage.vue        // Editar usuario
│   ├── RolesManagementPage.vue // Gestión de roles
│   ├── AuditLogsPage.vue       // Logs de auditoría
│   └── SystemSettingsPage.vue  // Configuración del sistema
```

---

## ⚙️ Módulo de Configuración Personal
*Configuraciones y perfil del usuario*

| Ruta | Componente | Descripción | Roles | Props/Estado | Datos API |
|------|------------|-------------|--------|--------------|-----------|
| `/profile` | `UserProfilePage` | Perfil del usuario actual | `ADMIN, EMPLOYEE` | - | `/auth/me` |
| `/profile/edit` | `EditProfilePage` | Editar perfil personal | `ADMIN, EMPLOYEE` | - | `/auth/me` |
| `/profile/password` | `ChangePasswordPage` | Cambiar contraseña | `ADMIN, EMPLOYEE` | - | - |
| `/profile/sessions` | `ActiveSessionsPage` | Sesiones activas | `ADMIN, EMPLOYEE` | - | `/auth/sessions` |
| `/settings` | `UserSettingsPage` | Configuraciones personales | `ADMIN, EMPLOYEE` | - | `/user/settings` |

### Componentes de Perfil:
```
src/
├── components/profile/
│   ├── ProfileCard.vue         // Tarjeta de perfil
│   ├── EditProfileForm.vue     // Formulario de edición
│   ├── ChangePasswordForm.vue  // Formulario cambio contraseña
│   ├── SessionsList.vue        // Lista de sesiones activas
│   └── UserPreferences.vue     // Preferencias del usuario
├── pages/profile/
│   ├── UserProfilePage.vue     // Perfil del usuario
│   ├── EditProfilePage.vue     // Editar perfil
│   ├── ChangePasswordPage.vue  // Cambiar contraseña
│   ├── ActiveSessionsPage.vue  // Sesiones activas
│   └── UserSettingsPage.vue    // Configuraciones
```

---

## 🔍 Rutas de Búsqueda Global
*Búsqueda transversal en todo el sistema*

| Ruta | Componente | Descripción | Roles | Props/Estado | Datos API |
|------|------------|-------------|--------|--------------|-----------|
| `/search` | `GlobalSearchPage` | Búsqueda global en el sistema | `ADMIN, EMPLOYEE` | `{query, filters}` | `/search/global` |
| `/search/products` | `ProductSearchResults` | Resultados de búsqueda de productos | `ADMIN, EMPLOYEE` | `{query}` | `/products/search` |
| `/search/suppliers` | `SupplierSearchResults` | Resultados de búsqueda de proveedores | `ADMIN, EMPLOYEE` | `{query}` | `/suppliers/search` |
| `/search/movements` | `MovementSearchResults` | Resultados de búsqueda de movimientos | `ADMIN, EMPLOYEE` | `{query}` | `/inventory/movements` |

---

## 📱 Rutas Móviles (PWA)
*Versión optimizada para dispositivos móviles*

| Ruta | Componente | Descripción | Roles | Props/Estado | Datos API |
|------|------------|-------------|--------|--------------|-----------|
| `/mobile` | `MobileDashboard` | Dashboard móvil | `ADMIN, EMPLOYEE` | - | `/dashboard/metrics` |
| `/mobile/scan` | `BarcodeScannerPage` | Escáner de códigos de barras | `ADMIN, EMPLOYEE` | - | `/products/code/:code` |
| `/mobile/quick-movement` | `QuickMovementPage` | Movimiento rápido móvil | `ADMIN, EMPLOYEE` | `{productCode}` | `/inventory/movements` |
| `/mobile/stock-check` | `MobileStockCheck` | Consulta rápida de stock | `ADMIN, EMPLOYEE` | - | `/inventory/stock` |

---

## 🛡️ Guards y Middleware

### Authentication Guards:
```javascript
// Rutas que requieren autenticación
const authGuard = (to, from, next) => {
  if (store.getters.isAuthenticated) {
    next();
  } else {
    next('/login');
  }
};

// Rutas solo para usuarios no autenticados
const publicOnlyGuard = (to, from, next) => {
  if (!store.getters.isAuthenticated) {
    next();
  } else {
    next('/dashboard');
  }
};

// Guard de roles
const roleGuard = (allowedRoles) => {
  return (to, from, next) => {
    const userRoles = store.getters.userRoles;
    const hasPermission = allowedRoles.some(role => userRoles.includes(role));
    
    if (hasPermission) {
      next();
    } else {
      next('/unauthorized');
    }
  };
};
```

### Middleware de Navegación:
- `AuthMiddleware`: Verificación de token JWT
- `RoleMiddleware`: Control de permisos por ruta
- `SessionMiddleware`: Validación de sesión activa
- `AuditMiddleware`: Log de navegación para auditoría

---

## 🗂️ Estructura de Carpetas Frontend

```
src/
├── components/           # Componentes reutilizables
│   ├── common/          # Componentes comunes (botones, modales, etc.)
│   ├── auth/            # Componentes de autenticación
│   ├── dashboard/       # Componentes del dashboard
│   ├── products/        # Componentes de productos
│   ├── inventory/       # Componentes de inventario
│   ├── categories/      # Componentes de categorías
│   ├── suppliers/       # Componentes de proveedores
│   ├── reports/         # Componentes de reportes
│   ├── admin/           # Componentes de administración
│   └── profile/         # Componentes de perfil
├── pages/               # Páginas completas
│   ├── auth/
│   ├── dashboard/
│   ├── products/
│   ├── inventory/
│   ├── categories/
│   ├── suppliers/
│   ├── reports/
│   ├── admin/
│   └── profile/
├── router/              # Configuración de rutas
│   ├── index.js         # Router principal
│   ├── guards.js        # Guards de autenticación
│   └── modules/         # Rutas por módulo
├── store/               # Estado global (Vuex/Redux)
│   ├── modules/
│   │   ├── auth.js
│   │   ├── products.js
│   │   ├── inventory.js
│   │   └── ...
├── services/            # Servicios API
│   ├── api.js           # Cliente HTTP base
│   ├── auth.service.js
│   ├── products.service.js
│   └── ...
├── utils/               # Utilidades
│   ├── constants.js
│   ├── helpers.js
│   ├── validators.js
│   └── formatters.js
└── assets/              # Recursos estáticos
    ├── css/
    ├── images/
    └── icons/
```

---

## 🎨 Estados y Navegación

### Estados Globales (Store):
- `auth`: Autenticación y usuario actual
- `products`: Lista y cache de productos
- `inventory`: Estado del inventario
- `notifications`: Alertas y notificaciones
- `ui`: Estado de la interfaz (loading, modales, etc.)

### Flujos de Navegación Principales:
1. **Login** → Dashboard → Módulos específicos
2. **Producto nuevo** → Categoría/Proveedor → Stock inicial
3. **Movimiento** → Selección producto → Confirmación → Actualización stock
4. **Reporte** → Selección tipo → Parámetros → Generación → Descarga

---