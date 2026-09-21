# 🎨 FossFLOW Docker - Creador de Diagramas Isométricos Autohospedado

[![GitHub Stars](https://img.shields.io/github/stars/stan-smith/FossFLOW?style=for-the-badge&logo=github)](https://github.com/stan-smith/FossFLOW)
[![Docker Pulls](https://img.shields.io/docker/pulls/stnsmith/fossflow?style=for-the-badge&logo=docker)](https://hub.docker.com/r/stnsmith/fossflow)
[![License](https://img.shields.io/github/license/stan-smith/FossFLOW?style=for-the-badge)](https://github.com/stan-smith/FossFLOW/blob/main/LICENSE)
[![Docker Image Size](https://img.shields.io/docker/image-size/stnsmith/fossflow/latest?style=for-the-badge&logo=docker)](https://hub.docker.com/r/stnsmith/fossflow)

## 📋 Descripción general

**FossFLOW** es una herramienta open source para crear hermosos diagramas isométricos de infraestructura completamente gratis, sin dependencias cloud, y sin límites de features. Es la alternativa gratuita y self-hosted a Cloudcraft (que cobra), permitiéndote visualizar arquitecturas de nube, infraestructura on-premise, y diseños de red de forma clara y profesional.

Propuesta clave: **PWA moderno (Progressive Web App)** que funciona offline en el navegador. Sin servidor requerido para funciones básicas, pero con opción de almacenamiento servidor para persistencia entre dispositivos. Todos los diagramas editables mediante drag-and-drop. Auto-save cada 5 segundos. Export/import JSON. Componentes isométricos 3D hermosos basados en isoflow library.

Arquitectura monorepo: `fossflow-lib` (React component library) + `fossflow-app` (PWA) + `fossflow-backend` (optional Express server storage). React + TypeScript. MIT open source.

## ✨ Características principales

- 🎯 **Diagrama isométrico drag-and-drop** - Interfaz intuitiva, coloca componentes con arrastrar/soltar
- 📱 **PWA con soporte offline** - Funciona completamente offline en navegador, edita sin internet
- ☁️ **Componentes pre-built** - AWS, Azure, GCP, infraestructura genérica. Biblioteca creciente
- 🖼️ **Importa tus propios íconos** - PNG, JPG, SVG custom con escalado automático
- 🔄 **Toggle isométrico/plano** - Cambia vista según necesidad
- 💾 **Auto-save cada 5 segundos** - Tus cambios siempre guardados, nunca pierdes trabajo
- 📤 **Export/Import JSON** - Comparte diagramas, versión control, portabilidad
- 🐳 **Almacenamiento en servidor Docker** - Diagrams persisten en filesystem, multi-dispositivo
- ⌨️ **Hotkeys configurables** - Perfiles: QWERTY (default), SMNRCT, None. Eficiencia
- 🌙 **Dark mode + Responsive** - UI moderna, desktop, tablet, móvil, accesible
- 🌍 **Multi-idioma** - Soporte múltiples idiomas, i18n integrado
- ⚡ **Zero dependencies (local)** - No requiere BD, Redis, u otros servicios para funcionar
- 📜 **Open source MIT** - Código abierto, gratis, comunidad, contribuye

## 📋 Requisitos del sistema

- 🐳 **Docker** (obligatorio)
- 🐙 **Docker Compose** (opcional, para almacenamiento servidor)
- 💾 **256 MB RAM mínimo** (muy ligero)
- 💿 **100 MB espacio disco** (para imagen Docker)
- 🔌 **Puerto 80 o custom** (para acceso web)
- 🌐 **Navegador moderno**: Chrome, Edge, Firefox, Safari
- 📱 **Soporte para PWA** (requerido para instalación app)
- ⚡ **Ultra-ligero**: PWA funciona sin servidor. Docker opcional solo para persistencia multi-dispositivo

## 🐳 Instalación

### Opción 1: Docker Compose (almacenamiento persistente) - **Recomendado**

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
```

```bash
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

### Acceso

```
http://localhost
```
*(o `http://localhost:PUERTO` si cambias el puerto)*

## ⚙️ Configuración

1. **ENABLE_SERVER_STORAGE** - `true` para persistencia en servidor (requiere volumen), `false` para solo localStorage del navegador
2. **STORAGE_PATH** - Ruta dentro del contenedor donde guardar diagramas (default: `/data/diagrams`)
3. **NODE_ENV** - `production` para optimizaciones, `development` para debug
4. **ENABLE_GIT_BACKUP** - `true` para activar backup automático a repositorio Git (requiere configuración adicional)
5. **Puerto host** - Cambia `"80:80"` por `"PUERTO_DESEADO:80"` en docker-compose.yml

## 🚀 Primeros pasos

1. **Explorar componentes disponibles** - Panel izquierdo: componentes (AWS, Azure, GCP, genéricos). Haz scroll para ver más. Click en componente para seleccionar. Click en canvas para colocar
2. **Crear primer diagrama simple (ejemplo AWS)** - Busca "EC2" o "Lambda" en panel. Arrastra al canvas. Arrastra "RDS" (database). Arrastra "S3" (storage). Click y arrastra elementos para mover. Posiciona en forma de arquitectura (tier: web, app, data)
3. **Agregar conexiones (líneas entre componentes)** - Busca "Connector" o línea en panel. Dibuja líneas conectando componentes. Crea diagrama de arquitectura clara
4. **Cambiar tema (Dark mode)** - Click ícono luna (moon) arriba a la derecha. Toggle dark/light mode. Preferencia guardada
5. **Agregar tus propios íconos** - Click "Upload Icon" en panel. Selecciona PNG/JPG/SVG. El ícono aparece en diagrama. Toggle isométrico/plano si quieres
6. **Hotkeys configurables** - Settings (engranaje) → Hotkeys. Elige perfil: QWERTY (default), SMNRCT, None. Mejora velocidad dibujo
7. **Export diagrama (JSON)** - Menú superior → "Export". Descarga .json. Comparte con equipo o guarda versión control
8. **Import diagrama (JSON)** - Menú superior → "Import". Selecciona .json previamente exportado. Se carga automáticamente
9. **Instalar como PWA (Mac/Linux/Chrome)** - URL bar → icono instalación (o menú) → "Install app". Se instala como app nativa. Icono en desktop/dock. Funciona offline

## 💡 Casos de uso

- 🏗️ **Arquitectos cloud**: Diseña AWS, Azure, GCP sin pagar Cloudcraft
- 🔧 **DevOps/SRE**: Visualiza infraestructura para documentación, onboarding
- 📚 **Documentación técnica**: Hermosos diagramas para proposals, presentaciones, wikis
- 🎨 **Design workshops**: Colaborativo (exporta/importa JSON). Iterate rápido
- 🏠 **Homelab**: Documenta tu infraestructura personal. Offline. Gratis

## 🔒 Acceso remoto seguro

### HTTPS con Caddy (producción)

```bash
# Caddyfile
diagrams.tudominio.com {
    reverse_proxy localhost:80
}
```

Acceso remoto seguro: `https://diagrams.tudominio.com` con HTTPS automático

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

MIT License - Código abierto, gratis, comunidad. Ver [LICENSE](https://github.com/stan-smith/FossFLOW/blob/main/LICENSE) para detalles.

---

> 📖 **Basado en el post**: [Cómo instalar FossFLOW en Docker - Creador de diagramas isométricos autohospedado en Docker](https://genbyte.blogspot.com/2026/07/como-insatlar-fossflow-en-docker.html)
>
> 🔗 **Referencias oficiales**:
> - [GitHub Original - Abrar74774/FossFLOW](https://github.com/Abrar74774/FossFLOW)
> - [GitHub Current Maintainer - stan-smith/FossFLOW](https://github.com/stan-smith/FossFLOW)
> - [Docker Hub - stnsmith/fossflow](https://hub.docker.com/r/stnsmith/fossflow)
> - [FossFLOW Documentation](https://fossflow.dev)
> - [Live Demo](https://demo.fossflow.dev)
> - [Isoflow Library](https://github.com/isoflow/isoflow)