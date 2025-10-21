# ✅ Huly Self-Host - Configuración Completada

## 🎉 Estado del Proyecto

¡La instalación de Huly se ha completado exitosamente! Todos los servicios están corriendo y listos para usar.

## 🌐 Acceso a la Aplicación

**URL Principal:** http://localhost:8087

Puedes acceder a Huly abriendo tu navegador web y visitando la URL anterior.

## 📦 Servicios en Ejecución

Todos los siguientes servicios están activos y funcionando:

- ✅ **nginx** - Proxy inverso (Puerto 8087)
- ✅ **front** - Frontend de Huly (Puerto 8081)
- ✅ **account** - Servicio de cuentas
- ✅ **transactor** - Servicio de transacciones
- ✅ **collaborator** - Servicio de colaboración
- ✅ **workspace** - Servicio de espacios de trabajo
- ✅ **fulltext** - Servicio de búsqueda de texto completo
- ✅ **stats** - Servicio de estadísticas
- ✅ **rekoni** - Servicio de procesamiento
- ✅ **kvs** - Almacenamiento clave-valor (Puerto 8094)
- ✅ **cockroach** - Base de datos CockroachDB
- ✅ **elastic** - Elasticsearch para búsqueda
- ✅ **minio** - Almacenamiento de objetos
- ✅ **redpanda** - Cola de mensajes

## 🔐 Configuración de Seguridad

### Secretos Generados

Se han generado automáticamente los siguientes archivos de secretos:

- `.huly.secret` - Secreto principal de Huly
- `.cr.secret` - Contraseña de CockroachDB
- `.rp.secret` - Contraseña de Redpanda

⚠️ **IMPORTANTE:** Estos archivos contienen información sensible. Mantenlos seguros y no los compartas.

### Credenciales de Base de Datos

**CockroachDB:**
- Usuario: `huly_user`
- Base de datos: `huly`
- Puerto interno: 26257
- ⚠️ **Modo Insecure**: Sin contraseña (solo para desarrollo/pruebas locales)

**MinIO:**
- Usuario: `minioadmin`
- Contraseña: `minioadmin`
- Puerto consola: 9001 (no expuesto públicamente)

**Redpanda:**
- Usuario admin: `admin`
- Puerto interno: 9092

## 📝 Variables de Entorno

Todas las variables de entorno están configuradas en el archivo `.env` en el directorio raíz del proyecto.

### Principales Variables:

- `HULY_VERSION=v0.7.242` - Versión de Huly instalada
- `HOST_ADDRESS=localhost:8087` - Dirección de acceso
- `DOCKER_NAME=huly_v7` - Nombre del proyecto Docker

## 🔧 Comandos Útiles

### Ver el estado de los servicios:
```bash
docker compose ps
```

### Ver logs de todos los servicios:
```bash
docker compose logs -f
```

### Ver logs de un servicio específico:
```bash
docker compose logs -f [nombre-servicio]
# Ejemplo: docker compose logs -f front
```

### Reiniciar todos los servicios:
```bash
docker compose restart
```

### Detener todos los servicios:
```bash
docker compose down
```

### Iniciar todos los servicios:
```bash
docker compose up -d
```

## 📊 Volúmenes de Datos

Los siguientes volúmenes Docker se han creado para almacenar datos persistentes:

- `huly_v7_elastic` - Índices de Elasticsearch
- `huly_v7_files` - Archivos subidos por usuarios
- `huly_v7_cr_data` - Datos de CockroachDB
- `huly_v7_cr_certs` - Certificados de CockroachDB
- `huly_v7_redpanda` - Datos de Redpanda

## 🚀 Próximos Pasos

1. Abre tu navegador en http://localhost:8087
2. Crea tu primera cuenta de usuario
3. Configura tu workspace

## 📚 Servicios Opcionales

Los siguientes servicios están disponibles pero no configurados por defecto:

- **Mail Service** - Para notificaciones por correo
- **Love Service** - Para llamadas de audio/video (requiere LiveKit)
- **AI Service** - Chatbot con IA (requiere OpenAI API)
- **Print Service** - Generación de PDFs
- **Calendar Service** - Integración con Google Calendar
- **GitHub Service** - Integración con GitHub

Consulta el `README.md` principal para instrucciones de configuración de estos servicios.

## 🔍 Resolución de Problemas

### Si un servicio no está funcionando:

1. Revisa los logs del servicio:
   ```bash
   docker compose logs [nombre-servicio]
   ```

2. Reinicia el servicio específico:
   ```bash
   docker compose restart [nombre-servicio]
   ```

3. Verifica la configuración en `.env`

### Si no puedes acceder a http://localhost:8087:

1. Verifica que nginx esté corriendo:
   ```bash
   docker compose ps nginx
   ```

2. Verifica que el puerto 8087 esté disponible:
   ```bash
   sudo netstat -tlnp | grep 8087
   ```

## 📞 Soporte

Para más información, consulta:
- README.md principal del proyecto
- Documentación oficial de Huly
- Repositorio: https://github.com/hcengineering/huly-selfhost

---

**Fecha de instalación:** 2025-10-21
**Versión instalada:** v0.7.242

