# 🎨 FossFLOW con Docker - Diagramas Isométricos 3D

![FossFLOW Logo](https://github.com/fossflow/fossflow/raw/main/logo.png) <!-- Placeholder: Replace with actual logo if available -->

> **FossFLOW** es una aplicación web de código abierto para crear diagramas isométricos (3D-style) de infraestructuras directamente en el navegador. Este repositorio proporciona un `docker-compose.yml` y un `README.md` en español para desplegar FossFLOW fácilmente usando Docker.

## 📖 Descripción

FossFLOW es una Progressive Web App (PWA) construida con React y basada en la librería Isoflow (fork de fosssflow). Funciona completamente en el navegador, incluso sin conexión, y prioriza la privacidad: los datos se guardan localmente (en `localStorage`) a menos que actives el almacenamiento en servidor.

Este proyecto simplifica el despliegue de FossFLOW mediante Docker Compose, incluyendo la configuración necesaria para el almacenamiento en servidor y la desactivación de respaldos Git automáticos (si no los necesitas).

## 🚀 Características

- ✅ **Diagramas isométricos 3D**: Crea infraestructuras bonitas y técnicas en perspectiva.
- ✅ **Autoguardado**: Guarda tu trabajo cada ~5 segundos.
- ✅ **Soporte PWA**: Puedes "instalarla" como aplicación nativa en Mac/Linux (y móviles).
- ✅ **Privacidad primero**: Por defecto, todo queda en tu navegador (localStorage).
- ✅ **Importar/exportar JSON**: Ideal para compartir o retomar después.
- ✅ **Guardado rápido de sesión**: Sin cuadros de diálogo pesados.
- ✅ **Modo offline completo**.
- ✅ **Almacenamiento en servidor opcional**: Cuando usas Docker y configuras `ENABLE_SERVER_STORAGE=true`.

## 📋 Requisitos del Sistema

- [Docker Engine](https://www.docker.com/get-started) (versión 20.10 o superior)
- [Docker Compose](https://docs.docker.com/compose/) (versión 2.0 o superior)
- Una distribución Linux (Ubuntu/Debian/etc.)
- Terminal y conexión a Internet

## 🛠️ Instalación

### Paso 1: Crear el archivo `docker-compose.yml`

Copia el siguiente contenido en un archivo llamado `docker-compose.yml`:

```yaml
version: '3.8'

services:
  fossflow:
    image: ghcr.io/fossflow/fossflow:latest
    container_name: fossflow
    environment:
      - ENABLE_SERVER_STORAGE=true
      - STORAGE_PATH=/data/diagrams
      - ENABLE_GIT_BACKUP=false
    volumes:
      - ./data:/data
    ports:
      - "8177:8177"
    restart: unless-stopped
```

### Paso 2: Crear el directorio de datos (opcional pero recomendado)

```bash
mkdir -p data
```

### Paso 3: Levantar el contenedor

```bash
docker compose up -d
```

Docker Compose descargará la imagen de FossFLOW y iniciará el contenedor con la configuración especificada.

### Paso 4: Verificar el despliegue

```bash
docker compose ps
```

Deberías ver el contenedor `fossflow` en estado `Up`.

### Paso 5: Acceder a FossFLOW

Abre tu navegador web y visita:

- Localmente: `http://localhost:8177/`
- En tu servidor: `http://TU_IP_DEL_SERVIDOR:8177/`

## 🔧 Configuración

### Variables de entorno

| Variable | Descripción | Valor por defecto |
|----------|-------------|-------------------|
| `ENABLE_SERVER_STORAGE` | Guarda diagramas en el servidor (no solo en el navegador) | `false` |
| `STORAGE_PATH` | Ruta interna donde FossFLOW escribe los JSON | `/data/diagrams` |
| `ENABLE_GIT_BACKUP` | Activa/desactiva el backup Git automático | `true` |

En el `docker-compose.yml` proporcionado:
- `ENABLE_SERVER_STORAGE=true` → Los diagramas se guardan en tu servidor (en el volumen `./data`).
- `STORAGE_PATH=/data/diagrams` → Los archivos JSON se almacenan en `./data/diagrams` en tu host.
- `ENABLE_GIT_BACKUP=false` → Desactiva el backup Git automático (útil si no tienes un repositorio Git configurado).

### Volúmenes de persistencia

- `./data:/data` - Almacena los diagramas JSON y cualquier otro dato persistente.

### Puertos expuestos

- `8177:8177` - Interfaz web de FossFLOW.

## 📖 Uso básico

Una vez que FossFLOW está en funcionamiento:

1. Accede a `http://localhost:8177/` (o la IP de tu servidor) en tu navegador.
2. Comienza a crear tu diagrama isométrico:
   - Haz clic en el botón "+" para añadir iconos.
   - Usa la herramienta de flecha para conectar elementos.
   - Añade texto con la herramienta de texto.
   - Usa cuadrados/rectangulares para agrupar o delimitar zonas.
3. Tu trabajo se guardará automáticamente cada ~5 segundos.
4. Para exportar tu diagrama:
   - **Export as JSON**: Para guardar un proyecto editable.
   - **Export as Compact JSON**: Versión más ligera del JSON.
   - **Export as image**: Para obtener un PNG listo para documentación.
   - **Open**: Para cargar un JSON previamente guardado.

## 📋 Mantenimiento

### Ver los logs

```bash
docker compose logs -f fossflow
```

### Reiniciar el servicio

```bash
docker compose restart
```

### Actualizar FossFLOW

```bash
docker compose pull
docker compose up -d
```

### Copia de seguridad de los diagramas

Simplemente copia el directorio `data`:

```bash
cp -r data ./backup/data-$(date +%Y%m%d)
```

### Restaurar la copia de seguridad

```bash
cp -r ./backup/data-YYYYMMDD/* data/
```

## 🛡️ Seguridad

- FossFLOW prioriza la privacidad: por defecto, todo se guarda en tu navegador.
- Si habilitas `ENABLE_SERVER_STORAGE`, asegúrate de que el directorio `data` tenga los permisos adecuados y esté protegido si contiene información sensible.
- Mantén actualizada la imagen de FossFLOW para obtener los últimos parches de seguridad.

## 📚 Recursos adicionales

- [Repositorio oficial de FossFLOW](https://github.com/fossflow/fossflow)
- [Documentación de Isoflow](https://github.com/isovalent/isolation) (librería base)
- [Guía de mejores prácticas para Docker](https://docs.docker.com/engine/security/#/docker-daemon-attack-surface)

## 🙏 Créditos

- Este `docker-compose.yml` y `README.md` fueron generados a partir del tutorial del blog de Genbyte: [Instalar FossFLOW con Docker: crea diagramas 3D isométricos tipo Excalidraw](https://genbyte.blogspot.com/2025/12/instalar-fossflow-con-docker-crea.html)
- Desarrollado por la comunidad de FossFLOW.
- Iconos e interfaz inspirados en Excalidraw y Isoflow.

## 📄 Licencia

Este proyecto está bajo la Licencia MIT - ver el archivo [LICENSE](LICENSE) para más detalles.

---

🚀 ¡Disfruta creando diagramas de infraestructura impresionantes con FossFLOW y Docker!