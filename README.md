# 🧾 Sistema de Gestión de Inventarios Stock Flow

Aplicación web para la gestión eficiente de inventarios, diseñada para pequeñas y medianas empresas. Permite el registro, control y seguimiento de productos, con soporte para múltiples roles y autenticación segura.

---

## 🚀 Funcionalidades

* Registro y actualización de productos
* Control de stock en tiempo real
* Historial de movimientos (entradas/salidas)
* Generación de reportes básicos
* Roles de acceso: **Administrador** y **Empleado**
* Autenticación y autorización segura con **JWT**

---

## 🛠️ Tecnologías

### 🔷 Frontend

* **[SvelteKit](https://kit.svelte.dev/):** Framework moderno y reactivo para construir interfaces web rápidas.
* **[Skeleton UI](https://www.skeleton.dev/):** Sistema de componentes UI para Svelte, responsivo y personalizable.

### 🔶 Backend

* **[Go](https://golang.org/) + [Fiber](https://gofiber.io/):** API rápida y minimalista escrita en Go, ideal para aplicaciones web eficientes.
* **[GORM](https://gorm.io/):** ORM para Go, facilita operaciones con la base de datos.
* **[JWT](https://jwt.io/):** Sistema de autenticación basado en tokens.

### 🗄️ Base de datos

* **SQLite3:** Base de datos ligera y embebida, ideal para aplicaciones pequeñas o medianas sin requerimientos de alto tráfico.

---

## 📦 Estructura general

```
/frontend       # Proyecto SvelteKit + Skeleton UI
/backend        # API Go con Fiber + GORM
/docs           # Documentacion y Diagramas
```

---

## 🔐 Seguridad

* Gestión de sesiones y accesos mediante JWT.
* Rutas protegidas según el rol del usuario (empleado o administrador).
* Validaciones del lado cliente y servidor.

---

## 🧪 Próximos pasos

* Mejoras en los reportes (gráficos, exportación a PDF/Excel)
* Integración con servicios externos (correo, notificaciones)
* Soporte multiempresa/multiusuario
