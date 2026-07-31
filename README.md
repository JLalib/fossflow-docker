# 🎨 FossFLOW Docker | [![GitHub](https://img.shields.io/badge/GitHub-fossflow--docker-blue?logo=github)](https://github.com/JLalib/fossflow-docker) [![Docker](https://img.shields.io/badge/Docker-stnsmith%2Ffossflow-blue?logo=docker)](https://hub.docker.com/r/stnsmith/fossflow) [![License](https://img.shields.io/badge/License-MIT-green)](https://opensource.org/licenses/MIT)

## 📋 Descripción general

FossFLOW es una herramienta **open source** para crear diagramas isométricos de infraestructura completamente gratis, sin dependencias cloud y sin límites de features. Es la alternativa gratuita y autohospedada a Cloudcraft, permitiéndote visualizar arquitecturas de nube, infraestructura on-premise y diseños de red de forma clara y profesional.

Funciona como una **PWA (Progressive Web App)** moderna con soporte offline completo en el navegador. No requiere servidor para funciones básicas, pero ofrece almacenamiento en servidor Docker para persistencia entre dispositivos.

## ✨ Características principales

- 🎯 **Diagramas isométricos drag-and-drop** – Interfaz intuitiva con componentes arrastrables
- 📱 **PWA con soporte offline** – Funciona completamente offline, instalable como app nativa
- ☁️ **Componentes pre-built** – AWS, Azure, GCP, infraestructura genérica (biblioteca creciente)
- 🖼️ **Importa tus propios íconos** – PNG, JPG, SVG con escalado automático
- 🔄 **Toggle isométrico/plano** – Cambia vista según necesidad
- 💾 **Auto-save cada 5 segundos** – Nunca pierdes trabajo
- 📤 **Export/Import JSON** – Comparte diagramas, control de versiones, portabilidad
- 🐳 **Almacenamiento en servidor Docker** – Diagramas persisten en filesystem, multi-dispositivo
- ⌨️ **Hotkeys configurables** – Perfiles QWERTY, SMNRCT, None
- 🌙 **Dark mode + Responsive** – Desktop, tablet, móvil, accesible
- 🌐 **Multi-idioma** – i18n integrado
- ⚡ **Zero dependencies** – No requiere BD, Redis u otros servicios
- 📄 **Open source MIT** – Código abierto, gratis, comunidad

## 📋 Requisitos del sistema

- Docker
- Docker Compose (opcional, para almacenamiento servidor)
- 256 MB RAM mínimo (muy ligero)
- 100 MB espacio disco (para imagen Docker)
- Puerto 80 o personalizado (para acceso web)
- Navegador moderno: Chrome, Edge, Firefox, Safari
- Soporte para PWA (requerido para instalación app)

> **Ultra-ligero**: PWA funciona sin servidor. Docker opcional solo para persistencia multi-dispositivo.

## 🐳 Instalación

### Opción 1: Docker Compose (almacenamiento persistente, recomendado)

```bash
cat > docker-compose.yml << 'EOF'
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
EOF

docker compose up -d
```

### Opción 2: Docker run simple (almacenamiento local navegador)

```bash
docker run -d \
  --name fossflow \
  --restart unless-stopped \
  -p 80:80 \
  -e ENABLE_SERVER_STORAGE=false \
  stnsmith/fossflow:latest
```

### Acceder

- **http://localhost** – Dashboard FossFLOW (o `http://localhost:PUERTO` si cambias el puerto)

## ⚙️ Configuración

Variables de entorno clave:

1. **ENABLE_SERVER_STORAGE** – `true` para almacenamiento en servidor (persistencia multi-dispositivo), `false` para solo localStorage del navegador
2. **STORAGE_PATH** – Ruta dentro del contenedor donde guardar diagramas (ej: `/data/diagrams`)
3. **NODE_ENV** – `production` para modo producción
4. **ENABLE_GIT_BACKUP** – `true`/`false` para habilitar backup automático a Git (opcional)

## 🚀 Primeros pasos

1. **Explorar componentes disponibles** – Panel izquierdo: componentes (AWS, Azure, GCP, genéricos). Haz scroll para ver más. Click en componente para seleccionar, click en canvas para colocar.
2. **Crear primer diagrama simple (ejemplo AWS)** – Busca "EC2" o "Lambda" en panel, arrastra al canvas. Arrastra "RDS" (database) y "S3" (storage). Click y arrastra elementos para mover. Posiciona en forma de arquitectura (tier: web, app, data).
3. **Agregar conexiones** – Busca "Connector" o línea en panel. Dibuja líneas conectando componentes. Crea diagrama de arquitectura clara.
4. **Cambiar tema (Dark mode)** – Click ícono luna (moon) arriba a la derecha. Toggle dark/light mode. Preferencia guardada.
5. **Agregar tus propios íconos** – Click "Upload Icon" en panel. Selecciona PNG/JPG/SVG. El ícono aparece en diagrama. Toggle isométrico/plano si quieres.
6. **Hotkeys configurables** – Settings (engranaje) → Hotkeys. Elige perfil: QWERTY (default), SMNRCT, None. Mejora velocidad dibujo.
7. **Exportar diagrama (JSON)** – Menú superior → "Export". Descarga .json. Comparte con equipo o guarda en control de versiones.
8. **Importar diagrama (JSON)** – Menú superior → "Import". Selecciona .json previamente exportado. Se carga automáticamente.
9. **Instalar como PWA** – Barra de URL → icono instalación (o menú) → "Install app". Se instala como app nativa. Icono en desktop/dock. Funciona offline.

## 💡 Casos de uso

- 🏗️ **Arquitectos cloud** – Diseña AWS, Azure, GCP sin pagar Cloudcraft
- ⚙️ **DevOps/SRE** – Visualiza infraestructura para documentación, onboarding
- 📚 **Documentación técnica** – Hermosos diagramas para proposals, presentaciones, wikis
- 🤝 **Design workshops** – Colaborativo (exporta/importa JSON). Iteración rápida
- 🏠 **Homelab** – Documenta tu infraestructura personal. Offline. Gratis

## 🔒 Acceso remoto seguro

```caddyfile
# Caddyfile
diagrams.tudominio.com {
    reverse_proxy localhost:80
}
```

Acceso remoto seguro: **https://diagrams.tudominio.com** con HTTPS automático.

> **Nota importante**: WebSocket requerido para HTTPS. Si usas Caddy con HTTPS, asegúrate que WebSocket está habilitado en reverse proxy.

## 🛠️ Gestión y mantenimiento

```bash
# Ver logs
docker compose logs -f fossflow

# Backup de diagramas
cp -r ./diagrams ./diagrams-backup-$(date +%Y%m%d)

# Restore de backup
rm -rf ./diagrams
cp -r ./diagrams-backup-YYYYMMDD ./diagrams
docker compose restart fossflow

# Reiniciar
docker compose restart fossflow

# Actualizar a versión más reciente
docker compose pull
docker compose up -d

# Monitorear consumo
docker stats fossflow
# Verás: mínimo CPU, ~50-100MB RAM

# Limpiar almacenamiento (opcional)
du -sh ./diagrams
# Ver tamaño total de diagramas almacenados
```

## 📝 Licencia

MIT License – Código abierto, gratis, comunidad. Ver [LICENSE](https://github.com/stan-smith/FossFLOW/blob/main/LICENSE) en el repositorio original.

---

> ✨ **Nota**: Este repositorio contiene la configuración Docker y documentación extraída del tutorial de Genbyte: <a href="https://genbyte.blogspot.com/2026/07/como-insatlar-fossflow-en-docker.html" target="_blank" rel="noopener noreferrer">Cómo instalar FossFLOW en Docker - Creador de diagramas isométricos autohospedado en Docker</a>