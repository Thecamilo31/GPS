# GPS - Plataforma de monitoreo vehicular

Sistema full‑stack para monitoreo de dispositivos GPS (GT06) con panel web en tiempo real, alertas, geozonas, reportes e historial de rutas. El frontend está en Vue 3 y el backend en Node.js/Express con MongoDB y WebSocket.

## Arquitectura

- **Frontend**: SPA en Vue 3 (Vue Router, Vuex), mapas con Leaflet, gráficos con Chart.js.
- **Backend**: API REST + servidor TCP para dispositivos GT06 + MQTT + WebSocket para notificaciones en tiempo real.
- **Base de datos**: MongoDB (Mongoose).

## Funcionalidades principales

- **Tiempo real**: seguimiento de posición y estado del dispositivo.
- **Alertas**: exceso de velocidad, frenado brusco, geozona, encendido/apagado, control remoto.
- **Geozonas**: polígonos y círculos con detección de entrada/salida.
- **Historial**: consulta de rutas por rango de fechas.
- **Reportes**: vistas y exportaciones (PDF) desde el frontend.

## Tecnologías

- Vue 3, Vue Router, Vuex
- Leaflet, Leaflet Draw, Leaflet Routing
- Chart.js, Chartkick
- Node.js, Express, MQTT, WebSocket
- MongoDB, Mongoose

## Requisitos

- Node.js 18+
- MongoDB (local o Atlas)
- Dispositivo compatible con protocolo **GT06** (opcional para pruebas)

## Estructura del proyecto

- Frontend: [gps/](gps)
- Backend: [gps/Back-end/](gps/Back-end)

## Configuración

### Backend (Node/Express)

Variables usadas por el backend (pueden configurarse en el entorno):

- `GT06_SERVER_PORT` (por defecto `4000`)
- `HTTP_PORT` (por defecto `80`)
- `MQTT_ROOT_TOPIC` (por defecto `gt06`)
- `MQTT_BROKER_URL`
- `MQTT_BROKER_PORT`
- `MQTT_BROKER_PROTO` (por defecto `mqtt`)
- `MQTT_BROKER_USER`
- `MQTT_BROKER_PASSWD`

El backend también contiene una cadena de conexión de MongoDB en [gps/Back-end/server.js](gps/Back-end/server.js). Recomendado moverla a variables de entorno antes de desplegar en producción.

### Frontend (Vue)

El WebSocket del frontend está definido en [gps/src/main.ts](gps/src/main.ts). Si cambias el host del backend, actualiza esa URL.

## Instalación

### 1) Frontend

Desde la carpeta [gps/](gps):

```
npm install
```

Para desarrollo:

```
npm run serve
```

Para build de producción:

```
npm run build
```

### 2) Backend

Desde la carpeta [gps/Back-end/](gps/Back-end):

```
npm install
```

Ejecutar servidor:

```
npm start
```

## Endpoints principales (resumen)

Base: `http://<host>:<HTTP_PORT>`

- `GET /devices` – lista de dispositivos
- `POST /devices` – crear dispositivo
- `PUT /devices/:id` – actualizar dispositivo
- `POST /devices/update-from-gps` – actualizar estado desde GPS
- `POST /devices/save-history` – guardar historial
- `GET /devices/history/:imei?startDate=...&endDate=...` – historial por rango
- `GET /notificaciones` – notificaciones
- `GET /geozone` – geozonas

## Notificaciones en tiempo real

El backend expone WebSocket en el mismo puerto HTTP. El frontend escucha eventos y muestra notificaciones con `iziToast` según el tipo (`maxSpeed`, `hardBraking`, `geozone`, `moto`, `control`).

## Docker (backend)

Existe un Dockerfile en [gps/Back-end/Dockerfile](gps/Back-end/Dockerfile). Desde esa carpeta:

```
docker build -t gps-backend .
docker run -p 80:80 -p 4000:4000 gps-backend
```

## Notas

- El servidor TCP para dispositivos GT06 escucha en `GT06_SERVER_PORT`.
- Si usas otro host para el backend, actualiza el WebSocket del frontend en [gps/src/main.ts](gps/src/main.ts).

## Licencia

Proyecto académico/interno. Ajusta la licencia si aplica.
