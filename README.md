# 📐 FossFLOW: Creador de diagramas isométricos autohospedado con Docker

[![GitHub](https://img.shields.io/badge/GitHub-Repositorio-blue)](https://github.com/JLalib/fossflow-docker) [![Docker](https://img.shields.io/badge/Docker-FossFLOW-blue)](https://hub.docker.com/r/stnsmith/fossflow) [![License](https://img.shields.io/badge/Licencia-MIT-green)](https://github.com/JLalib/fossflow-docker/blob/main/LICENSE)

## 📋 Descripción general

FossFLOW es una herramienta open source para crear hermosos diagramas isométricos de infraestructura completamente gratis, sin dependencias cloud y sin límites de funcionalidades. Es la alternativa gratuita y self-hosted a Cloudcraft, permitiéndote visualizar arquitecturas de nube, infraestructura on-premise y diseños de red de forma clara y profesional.

Este repositorio contiene la configuración necesaria para desplegar FossFLOW con Docker Compose, siguiendo el tutorial de Genbyte para dibujar tu infraestructura sin depender de servicios de pago.

## ✨ Características principales

- **Diagramas isométricos drag-and-drop**: interfaz intuitiva, coloca componentes arrastrando y soltando
- **PWA con soporte offline completo**: funciona sin conexión directamente desde el navegador
- **Componentes pre-construidos**: AWS, Azure, GCP e infraestructura genérica, con biblioteca en crecimiento
- **Importa tus propios íconos**: PNG, JPG y SVG personalizados, con escalado automático
- **Auto-save cada 5 segundos**: tus cambios siempre guardados, nunca pierdes trabajo
- **Export/Import JSON**: comparte diagramas, versiona y garantiza portabilidad total
- **Almacenamiento en servidor**: los diagramas persisten en el filesystem del contenedor Docker, con soporte multi-dispositivo
- **Hotkeys configurables**: perfiles QWERTY, SMNRCT o ninguno, para mayor eficiencia
- **Dark mode + diseño responsive**: UI moderna adaptada a desktop, tablet y móvil
- **Multi-idioma**: soporte i18n integrado
- **MIT open source**: código abierto, gratuito y con comunidad activa

## 📋 Requisitos del sistema

- Docker y Docker Compose (opcional, solo para almacenamiento en servidor)
- Al menos 256 MB de RAM (muy ligero)
- 100 MB de espacio en disco para la imagen Docker
- Puerto 80 disponible (o el que elijas) para el acceso web
- Navegador moderno: Chrome, Edge, Firefox o Safari
- Soporte para PWA si quieres instalarlo como app

💡 Ultra-ligero: FossFLOW funciona sin servidor usando el almacenamiento local del navegador; Docker es opcional y solo necesario para persistencia multi-dispositivo.

## 🐳 Instalación

### Opción 1: Docker Compose (almacenamiento persistente, recomendado)

Crea un archivo `docker-compose.yml` con el siguiente contenido:

```yaml
version: '3.8'

services:
  fossflow:
    image: stnsmith/fossflow:latest
    container_name: fossflow
    restart: unless-stopped
    ports:
      - "80:80"
    volumes:
      - ./diagrams:/data/diagrams
    environment:
      # Almacenamiento en servidor (recomendado)
      - ENABLE_SERVER_STORAGE=true
      - STORAGE_PATH=/data/diagrams
      # Modo producción
      - NODE_ENV=production
      # Backup a Git (opcional)
      - ENABLE_GIT_BACKUP=false
    healthcheck:
      test: ["CMD-SHELL", "nc -z 127.0.0.1 80 || exit 1"]
      interval: 10s
      timeout: 5s
      retries: 3
```

Luego, inicia el servicio:

```bash
docker compose up -d
```

### Opción 2: Docker run simple (almacenamiento local del navegador)

```bash
docker run -d \
  --name fossflow \
  --restart unless-stopped \
  -p 80:80 \
  -e ENABLE_SERVER_STORAGE=false \
  stnsmith/fossflow:latest
```

### Acceder

`http://localhost` - Dashboard de FossFLOW (o `http://localhost:PUERTO` si cambias el puerto)

## ⚙️ Configuración

Antes de iniciar el contenedor, revisa estas variables en tu `docker-compose.yml`:

1. **ENABLE_SERVER_STORAGE**: activa el almacenamiento persistente en el servidor (recomendado para multi-dispositivo)
2. **STORAGE_PATH**: ruta interna donde se guardan los diagramas (por defecto: `/data/diagrams`)
3. **NODE_ENV**: modo de ejecución, usa `production` para despliegues reales
4. **ENABLE_GIT_BACKUP**: activa backup automático a un repositorio Git (opcional)

💡 Consejo: si solo lo vas a usar en un dispositivo, puedes prescindir de Docker y usar la PWA directamente en el navegador con almacenamiento local.

## 🚀 Primeros pasos

1. Asegúrate de tener Docker y Docker Compose instalados si quieres almacenamiento persistente
2. Crea el `docker-compose.yml` y ejecuta `docker compose up -d`
3. Abre tu navegador en `http://localhost`
4. Verás el editor isométrico en blanco
5. Explora los componentes disponibles en el panel izquierdo (AWS, Azure, GCP, genéricos)
6. Crea tu primer diagrama:
   - Arrastra componentes como "EC2", "RDS" o "S3" al canvas
   - Posiciona los elementos en forma de arquitectura (web, app, datos)
   - Añade conectores para dibujar las relaciones entre componentes
7. Cambia a modo oscuro con el icono de luna arriba a la derecha
8. Sube tus propios íconos personalizados desde "Upload Icon"
9. Configura tus hotkeys favoritos en Settings → Hotkeys
10. Exporta tu diagrama en JSON para compartirlo o guardarlo en control de versiones
11. Instala FossFLOW como app nativa (PWA) desde el icono de instalación en la barra de URL

## 💡 Casos de uso

- **Arquitectos cloud**: diseña arquitecturas AWS, Azure o GCP sin pagar por Cloudcraft
- **DevOps/SRE**: visualiza infraestructura para documentación y onboarding de equipos
- **Documentación técnica**: diagramas profesionales para propuestas, presentaciones y wikis
- **Design workshops**: colaboración mediante export/import JSON, iteración rápida
- **Homelab**: documenta tu infraestructura personal de forma gratuita y offline

## 🔒 Acceso remoto seguro (opcional)

Si deseas acceder a FossFLOW desde fuera de tu red local de forma segura, puedes usar un proxy inverso como Caddy, Nginx Proxy Manager o Traefik para obtener un certificado gratuito de Let's Encrypt.

### Configuración Caddyfile (ejemplo)

```
diagrams.tudominio.com {
    reverse_proxy localhost:80
}
```

### Resultado

Acceso mediante `https://diagrams.tudominio.com` con HTTPS automático.

📝 Nota importante: si usas Caddy con HTTPS, asegúrate de que el WebSocket esté habilitado en el proxy inverso.

## 🛠️ Gestión y mantenimiento

### Ver logs

```bash
docker compose logs -f fossflow
```

### Backup de diagramas

```bash
cp -r ./diagrams ./diagrams-backup-$(date +%Y%m%d)
```

### Restaurar un backup

```bash
rm -rf ./diagrams
cp -r ./diagrams-backup-YYYYMMDD ./diagrams
docker compose restart fossflow
```

### Reiniciar el servicio

```bash
docker compose restart fossflow
```

### Actualizar a la última versión

```bash
docker compose pull
docker compose up -d
```

### Monitorear consumo

```bash
docker stats fossflow
# Verás: mínimo CPU, ~50-100MB RAM
```

## 📝 Licencia

Este proyecto se basa en [FossFLOW](https://github.com/stan-smith/FossFLOW), licenciado bajo MIT. La configuración y documentación proporcionada aquí está bajo la [MIT License](https://github.com/JLalib/fossflow-docker/blob/main/LICENSE).

---

> ✨ **Nota**: Este repositorio contiene la configuración Docker y documentación extraída del tutorial de Genbyte: <a href="https://genbyte.blogspot.com/2026/07/como-insatlar-fossflow-en-docker.html" target="_blank" rel="noopener noreferrer">Cómo instalar FossFLOW en Docker - Creador de diagramas isométricos autohospedado en Docker</a>
