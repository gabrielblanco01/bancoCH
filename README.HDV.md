# 💰 Banco Diario — Sistema de Gestión de Préstamos

![Java](https://img.shields.io/badge/Java-ED8B00?style=for-the-badge&logo=java&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-316192?style=for-the-badge&logo=postgresql&logoColor=white)
![MongoDB](https://img.shields.io/badge/MongoDB-4EA94B?style=for-the-badge&logo=mongodb&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)
![Railway](https://img.shields.io/badge/Railway-131415?style=for-the-badge&logo=railway&logoColor=white)

> Sistema web de gestión integral de préstamos desarrollado como proyecto académico en la Universidad Autónoma del Caribe (UAC). Permite administrar solicitudes de crédito, pagos, cobranza e incidencias, con roles diferenciados por tipo de usuario.

---

## 📋 Tabla de Contenidos

- [Descripción](#-descripción)
- [Tecnologías](#-tecnologías)
- [Arquitectura](#-arquitectura)
- [Funcionalidades](#-funcionalidades)
- [Roles del sistema](#-roles-del-sistema)
- [Base de datos](#-base-de-datos)
- [Instalación local](#-instalación-local)
- [Deploy en Railway](#-deploy-en-railway)
- [Credenciales de prueba](#-credenciales-de-prueba)

---

## 📌 Descripción

**Banco Diario** es una aplicación web empresarial construida en Java EE (Jakarta EE) que gestiona el ciclo completo de un préstamo: desde la solicitud y evaluación crediticia, hasta el seguimiento de pagos, cobranza y notificaciones. Utiliza **PostgreSQL** como motor de base de datos relacional y **MongoDB** para el registro de logs y auditoría de operaciones.

El sistema está preparado para producción, contenerizado con **Docker** y desplegable en la nube mediante **Railway**.

---

## 🛠 Tecnologías

| Capa | Tecnología |
|---|---|
| Backend | Java EE / Jakarta EE (NetBeans) |
| Base de datos relacional | PostgreSQL |
| Base de datos NoSQL (logs) | MongoDB |
| Frontend | JSF / HTML + CSS |
| Contenerización | Docker |
| Deploy en la nube | Railway |
| Control de versiones | Git / GitHub |

---

## 🏗 Arquitectura

```
┌─────────────────────────────────────────┐
│              Cliente Web                │
│         (Navegador / HTML+CSS)          │
└──────────────────┬──────────────────────┘
                   │
┌──────────────────▼──────────────────────┐
│          Servidor Java EE               │
│     (Lógica de negocio / Servlets)      │
└──────┬───────────────────────┬──────────┘
       │                       │
┌──────▼──────┐       ┌────────▼────────┐
│ PostgreSQL  │       │    MongoDB      │
│  (Datos     │       │  (Logs y        │
│  relaciona- │       │   auditoría)    │
│  les)       │       │                 │
└─────────────┘       └─────────────────┘
```

---

## ✅ Funcionalidades

- **Gestión de usuarios** con roles diferenciados y control de acceso
- **Solicitud de préstamos** con validación de montos y plazos configurables
- **Evaluación crediticia** por parte del analista de crédito
- **Seguimiento de pagos** con estados (pendiente, pagado, vencido)
- **Módulo de cobranza** para gestión de carteras en mora
- **Sistema de notificaciones** internas por usuario
- **Gestión de incidencias** técnicas (soporte)
- **Configuración global** del sistema (tasa de interés, montos mínimos/máximos, plazos)
- **Registro de logs** en MongoDB para auditoría de operaciones

---

## 👥 Roles del Sistema

| Rol | Descripción |
|---|---|
| `Administrador` | Acceso total al sistema y configuración |
| `Prestamista` | Gestión de préstamos activos |
| `Prestatario` | Solicita préstamos y consulta su estado |
| `AnalistaCredito` | Evalúa y aprueba/rechaza solicitudes |
| `AgenteCobranza` | Gestiona casos de mora y pagos pendientes |
| `SoporteTecnico` | Gestión de incidencias técnicas |

---

## 🗄 Base de Datos

El esquema PostgreSQL incluye las siguientes tablas:

```
rol → usuario → solicitud → prestamo → pago
                          ↘ evaluacion
                 ↘ cobranza
                 ↘ incidencia
                 ↘ notificacion
configuracion (parámetros globales del sistema)
```

**Parámetros por defecto:**
- Tasa de interés: 2.5%
- Plazo máximo: 24 meses
- Monto mínimo: $100,000 COP
- Monto máximo: $2,000,000 COP

El script completo está en [`banco_diario.sql`](./banco_diario.sql).

---

## 💻 Instalación Local

### Requisitos previos

- Java JDK 11 o superior
- NetBeans IDE 12+
- PostgreSQL 13+
- MongoDB 5+
- GlassFish / Payara Server

### Pasos

```bash
# 1. Clonar el repositorio
git clone https://github.com/gabrielblanco01/BANCO-DIARIO.git
cd BANCO-DIARIO

# 2. Crear la base de datos en PostgreSQL
psql -U postgres -c "CREATE DATABASE banco_diario;"
psql -U postgres -d banco_diario -f banco_diario.sql

# 3. Configurar la conexión en NetBeans
# Editar src/java/config/DatabaseConfig.java con tus credenciales locales

# 4. Abrir el proyecto en NetBeans
# File → Open Project → seleccionar carpeta BANCO-DIARIO

# 5. Clean and Build (Shift + F11)

# 6. Desplegar en GlassFish / Payara
# Run → Deploy
```

---

## 🚀 Deploy en Railway

El proyecto está listo para despliegue en la nube sin configuración adicional.

### 1. Crear proyecto en Railway

1. Ir a [railway.app](https://railway.app) y conectar con GitHub
2. **New Project → Deploy from GitHub repo** → seleccionar `BANCO-DIARIO`
3. Railway detecta el `Dockerfile` automáticamente ✅

### 2. Agregar PostgreSQL

1. **+ New → Database → PostgreSQL**
2. Las variables se generan automáticamente: `DATABASE_URL`, `PGUSER`, `PGPASSWORD`
3. Ir a la pestaña **Query** y ejecutar el contenido de `banco_diario.sql`

### 3. Agregar MongoDB

1. **+ New → Database → MongoDB**
2. Copiar la variable `MONGO_URL`
3. En el servicio Java → **Variables** → agregar `MONGO_URL`

### 4. Generar dominio público

1. Servicio Java → **Settings → Networking → Generate Domain**
2. Acceder desde cualquier navegador:

```
https://banco-diario-production.up.railway.app/login
```

---

## 🔑 Credenciales de Prueba

| Campo | Valor |
|---|---|
| Email | `admin@bancodiario.com` |
| Contraseña | `admin123` |
| Rol | Administrador |

> ⚠️ Cambiar las credenciales por defecto antes de usar en producción real.

---

## 👨‍💻 Autor

**Gabriel Estevan Blanco Ahumada**
Estudiante de Ingeniería de Sistemas — 8.° Semestre
Universidad Autónoma del Caribe, Barranquilla — Colombia

[![GitHub](https://img.shields.io/badge/GitHub-gabrielblanco01-181717?style=flat&logo=github)](https://github.com/gabrielblanco01)
[![Email](https://img.shields.io/badge/Email-g.blanco4515@hotmail.com-0078D4?style=flat&logo=microsoft-outlook)](mailto:g.blanco4515@hotmail.com)

---

> Proyecto desarrollado con fines académicos en la UAC. 2024-2025.
