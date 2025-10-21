# 🚀 Guía de Despliegue en Coolify

Esta guía te ayudará a desplegar Huly en Coolify.

## 📋 Requisitos Previos

- Coolify instalado y funcionando
- Un dominio configurado (ej: `huly.tudominio.com`)
- Mínimo 4GB de RAM en tu servidor

## 📝 Paso 1: Preparar el Proyecto

1. **Copia el archivo adaptado**: `compose.coolify.yml`
2. **Renómbralo a**: `compose.yml` (o déjalo con ese nombre y especifica el archivo en Coolify)

## 🔧 Paso 2: Crear el Proyecto en Coolify

### A. Crear Nuevo Proyecto

1. En Coolify, ve a **"Projects"** → **"New Project"**
2. Nombre: `huly-selfhost`
3. Tipo: **Docker Compose**

### B. Configurar el Compose

1. Sube o pega el contenido de `compose.coolify.yml`
2. Asegúrate de que esté seleccionado correctamente

## 🌐 Paso 3: Configurar el Dominio

### En Coolify:

1. Ve a la configuración del servicio **`front`**
2. En la sección **"Domains"**:
   - Dominio principal: `huly.tudominio.com`
   - Habilita **HTTPS automático** (Let's Encrypt)

### Configurar Paths adicionales:

Coolify necesita hacer proxy a los diferentes servicios. Configura estos paths en Coolify:

**Servicio `front`**:
- Path: `/` → Puerto: `8080`

**Servicio `account`**:
- Path: `/_accounts` → Puerto: `3000`

**Servicio `transactor`**:
- Path: `/_transactor` → Puerto: `3333` (WebSocket habilitado)

**Servicio `collaborator`**:
- Path: `/_collaborator` → Puerto: `3078` (WebSocket habilitado)

**Servicio `stats`**:
- Path: `/_stats` → Puerto: `4900`

**Servicio `rekoni`**:
- Path: `/_rekoni` → Porto: `4004`

## 🔐 Paso 4: Configurar Variables de Entorno

En Coolify, ve a **Environment Variables** y añade todas las variables del archivo `COOLIFY_ENV_TEMPLATE.txt`.

### ⚠️ IMPORTANTE - Reemplazar valores:

```env
# Reemplaza TU_DOMINIO con tu dominio real:
HOST_ADDRESS=huly.tudominio.com
FRONT_URL=https://huly.tudominio.com
TRANSACTOR_URL=ws://transactor:3333;wss://huly.tudominio.com

# Genera nuevos secretos (ejecuta 3 veces):
# openssl rand -hex 32
```

## 🗄️ Paso 5: Configurar Volúmenes Persistentes

Coolify maneja los volúmenes automáticamente, pero verifica que estos volúmenes estén creados:

- `elastic` - Datos de Elasticsearch
- `files` - Archivos subidos
- `cr_data` - Base de datos CockroachDB
- `redpanda` - Cola de mensajes

## 🚀 Paso 6: Desplegar

1. En Coolify, haz clic en **"Deploy"**
2. Espera a que todos los servicios se inicien (puede tomar 2-3 minutos)
3. Monitorea los logs para verificar que no haya errores

## ✅ Paso 7: Inicializar Base de Datos

Una vez desplegado, necesitas crear el usuario y la base de datos en CockroachDB:

```bash
# Conéctate al contenedor de cockroach en Coolify
docker exec -it <cockroach-container-id> /cockroach/cockroach sql --insecure

# Ejecuta estos comandos SQL:
CREATE USER IF NOT EXISTS huly_user;
CREATE DATABASE IF NOT EXISTS huly;
GRANT ALL ON DATABASE huly TO huly_user;
\q
```

O desde la terminal de Coolify del servicio cockroach:

```bash
/cockroach/cockroach sql --insecure -e "CREATE USER IF NOT EXISTS huly_user; CREATE DATABASE IF NOT EXISTS huly; GRANT ALL ON DATABASE huly TO huly_user;"
```

## 🔍 Paso 8: Verificar el Despliegue

1. Visita tu dominio: `https://huly.tudominio.com`
2. Deberías ver la pantalla de inicio de sesión de Huly
3. Haz clic en **"Sign Up"** para crear tu primera cuenta

## 🔧 Configuración Adicional en Coolify

### Configurar Proxy (si usa Traefik/Caddy)

Si Coolify usa Traefik, asegúrate de que las rutas WebSocket estén habilitadas:

```yaml
# En las labels de los servicios que usan WebSocket:
traefik.http.middlewares.ws-header.headers.customrequestheaders.Upgrade=websocket
traefik.http.middlewares.ws-header.headers.customrequestheaders.Connection=upgrade
```

### Recursos Recomendados

Configura los límites de recursos en Coolify:

**Elasticsearch**:
- Memory: 2GB
- CPU: 1 core

**CockroachDB**:
- Memory: 1GB
- CPU: 1 core

**Otros servicios**:
- Memory: 512MB cada uno
- CPU: 0.5 core cada uno

## 🐛 Solución de Problemas

### Error: "Connection refused to transactor"

**Solución**: Verifica que la variable `TRANSACTOR_URL` incluya ambas URLs:
```env
TRANSACTOR_URL=ws://transactor:3333;wss://huly.tudominio.com
```

### Error: "Failed to connect to database"

**Solución**: Verifica que el usuario y la base de datos se crearon correctamente en CockroachDB.

### WebSocket no funciona

**Solución**: 
1. Verifica que el proxy de Coolify tenga habilitado WebSocket
2. Asegúrate de que las rutas `/_transactor` y `/_collaborator` apunten a los servicios correctos
3. Verifica que el protocolo sea `wss://` (con SSL)

### Servicios no se inician

**Solución**: Revisa los logs en Coolify:
1. Ve a cada servicio
2. Mira los logs en tiempo real
3. Busca errores de conexión o configuración

## 📚 Diferencias con la Instalación Local

| Aspecto | Local (Docker Compose) | Coolify |
|---------|------------------------|---------|
| Proxy | Nginx interno | Traefik/Caddy de Coolify |
| SSL | Manual | Automático (Let's Encrypt) |
| Dominio | localhost:8087 | Tu dominio real |
| WebSocket | ws:// | wss:// (SSL) |
| Variables | Archivo .env | UI de Coolify |

## 🎯 Próximos Pasos

Una vez que Huly esté funcionando en Coolify:

1. Crea tu primera cuenta de administrador
2. Configura tu workspace
3. Invita a tu equipo
4. (Opcional) Configura servicios adicionales:
   - Servicio de correo (SMTP)
   - Notificaciones push
   - Integración con GitHub
   - Videollamadas (LiveKit)

## 📞 Soporte

Si encuentras problemas:

1. Revisa los logs de cada servicio en Coolify
2. Verifica que todas las variables de entorno estén correctas
3. Asegúrate de que el dominio esté resolviendo correctamente
4. Verifica que el SSL esté funcionando

---

**Nota**: Esta configuración usa CockroachDB en modo insecure, lo cual es aceptable para desarrollo pero para producción deberías configurar SSL en CockroachDB.

