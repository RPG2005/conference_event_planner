# EventOne - Conference Expense Planner 

Proyecto React desarrollado con Vite para planificar gastos de eventos y conferencias.

La aplicación muestra una página inicial de presentación y permite acceder al planificador mediante el botón **Get Started**.

coding-project-template

---

## Tecnologías usadas

- React 18
- Vite 5
- Redux Toolkit
- React Redux
- ESLint
- Docker
- Docker Compose

---

## Requisitos previos

Antes de arrancar el proyecto necesitas tener instalado:

- Node.js, recomendado versión 20 o superior
- npm
- Docker Desktop, si vas a trabajar con Docker
- Git, si vas a clonar o subir cambios al repositorio

Para comprobar que tienes Node y npm instalados:

```bash
node -v
npm -v
```

Para comprobar Docker:

```bash
docker --version
docker compose version
```

---

## Estructura básica del proyecto

```text
conference_event_planner/
├── Dockerfile
├── compose.yml
├── package.json
├── package-lock.json
├── index.html
├── vite.config.js
├── README.md
└── src/
    ├── main.jsx
    ├── App.jsx
    ├── App.css
    ├── index.css
    ├── store.js
    ├── ConferenceEvent.jsx
    └── AboutUs.jsx
```

---

## Arrancar el proyecto con Docker

### 1. Asegúrate de que Docker Desktop está abierto

En Windows, antes de ejecutar cualquier comando Docker, abre **Docker Desktop** y espera a que aparezca como iniciado.

Puedes comprobarlo con:

```bash
docker info
```

Si Docker no está iniciado, comandos como `docker compose up --build` fallarán.

---

### 2. Construir y levantar el proyecto

Desde la carpeta raíz del proyecto:

```bash
docker compose up --build
```

Después abre el navegador en:

```text
http://localhost:5173
```

---

### 3. Arrancar sin reconstruir

Cuando ya se ha construido una vez:

```bash
docker compose up
```

---

### 4. Arrancar en segundo plano

```bash
docker compose up -d
```

---

### 5. Ver logs del contenedor

```bash
docker compose logs -f
```

---

### 6. Parar el proyecto

```bash
docker compose down
```

---

### 7. Reconstruir desde cero

Útil si cambias el `Dockerfile`, `compose.yml` o hay problemas con dependencias:

```bash
docker compose down
docker compose up --build
```

---

## Archivos Docker recomendados

### Dockerfile

```dockerfile
FROM node:20-alpine

WORKDIR /app

COPY package*.json ./

RUN npm install

COPY . .

EXPOSE 5173

CMD ["npm", "run", "dev", "--", "--host", "0.0.0.0"]
```

---

### compose.yml

```yaml
services:
  react-app:
    build: .
    container_name: conference_event_planner-react-app
    ports:
      - "5173:5173"
    volumes:
      - .:/app
      - /app/node_modules
    environment:
      - CHOKIDAR_USEPOLLING=true
    command: npm run dev -- --host 0.0.0.0
```

---

### .dockerignore

```dockerignore
node_modules
dist
.git
.gitignore
npm-debug.log
Dockerfile
compose.yml
README.md
```

---

## Arrancar el proyecto sin Docker

Si prefieres ejecutar el proyecto directamente en tu máquina:

### 1. Instalar dependencias

```bash
npm install
```

---

### 2. Arrancar servidor de desarrollo

```bash
npm run dev
```

Después abre:

```text
http://localhost:5173
```

---

## Scripts disponibles

Los scripts principales están definidos en `package.json`.

### Desarrollo

```bash
npm run dev
```

Arranca Vite en modo desarrollo.

---

### Build de producción

```bash
npm run build
```

Genera la versión optimizada del proyecto dentro de la carpeta `dist`.

---

### Vista previa de producción

```bash
npm run preview
```

Construye el proyecto y arranca una vista previa de la versión generada.

---

### Linter

```bash
npm run lint
```

Ejecuta ESLint para revisar errores o advertencias en el código.

---

## Comandos útiles con Docker

### Ver contenedores activos

```bash
docker ps
```

---

### Ver todos los contenedores

```bash
docker ps -a
```

---

### Entrar dentro del contenedor

```bash
docker exec -it conference_event_planner-react-app sh
```

---

### Instalar una dependencia dentro del contenedor

```bash
docker exec -it conference_event_planner-react-app npm install nombre-paquete
```

Ejemplo:

```bash
docker exec -it conference_event_planner-react-app npm install axios
```

---

### Borrar contenedores parados

```bash
docker container prune
```

---

### Borrar imágenes no usadas

```bash
docker image prune
```

---

### Borrar volúmenes no usados

```bash
docker volume prune
```

---

## Comandos útiles de npm

### Instalar dependencias

```bash
npm install
```

---

### Instalar una dependencia

```bash
npm install nombre-paquete
```

Ejemplo:

```bash
npm install axios
```

---

### Instalar una dependencia de desarrollo

```bash
npm install -D nombre-paquete
```

Ejemplo:

```bash
npm install -D eslint
```

---

### Actualizar dependencias

```bash
npm update
```

---

## Comandos útiles de Git

### Ver estado del repositorio

```bash
git status
```

---

### Añadir cambios

```bash
git add .
```

---

### Crear commit

```bash
git commit -m "Descripción del cambio"
```

---

### Subir cambios

```bash
git push
```

---

### Ver ramas

```bash
git branch
```

---

### Crear una rama nueva

```bash
git checkout -b nombre-rama
```

---

## Problemas comunes

### Error: Docker no encuentra `dockerDesktopLinuxEngine`

Ejemplo de error:

```text
open //./pipe/dockerDesktopLinuxEngine: El sistema no puede encontrar el archivo especificado
```

Este error normalmente significa que **Docker Desktop no está abierto** o que el motor Linux de Docker no ha arrancado correctamente.

Solución:

1. Abre Docker Desktop.
2. Espera a que termine de iniciar.
3. Ejecuta:

```bash
docker info
```

4. Si funciona, vuelve a ejecutar:

```bash
docker compose up --build
```

Si sigue fallando, reinicia Docker Desktop o reinicia Windows.

---

### Error: el puerto 5173 ya está ocupado

Si aparece un error indicando que el puerto está en uso, puede haber otro proceso usando Vite.

Puedes cambiar el puerto en `compose.yml`:

```yaml
ports:
  - "5174:5173"
```

Entonces accederías desde:

```text
http://localhost:5174
```

---

### Error: cambios en archivos no se reflejan

En Windows puede fallar la detección automática de cambios dentro de Docker. Por eso se usa:

```yaml
environment:
  - CHOKIDAR_USEPOLLING=true
```

Si sigue fallando, reinicia el contenedor:

```bash
docker compose down
docker compose up
```

---

### Error: faltan dependencias

Si aparece un error de módulos no encontrados, reconstruye el contenedor:

```bash
docker compose down
docker compose up --build
```

O instala dependencias localmente:

```bash
npm install
```

---

## Flujo recomendado de trabajo

### Con Docker

```bash
docker compose up
```

Edita los archivos desde tu editor habitual y Vite debería recargar la página automáticamente.

---

### Sin Docker

```bash
npm install
npm run dev
```

---

## URL de desarrollo

```text
http://localhost:5173
```

---

## Notas importantes

- El proyecto usa Vite, por lo que el puerto por defecto es `5173`.
- Para Docker es importante arrancar Vite con `--host 0.0.0.0`.
- No subas `node_modules` al repositorio.
- La carpeta `dist` se genera automáticamente al ejecutar `npm run build`.
- Si trabajas en Windows, se recomienda usar Docker Desktop abierto antes de lanzar `docker compose`.

---

## Resumen rápido

```bash
# Arrancar con Docker
docker compose up --build

# Parar Docker
docker compose down

# Arrancar localmente
npm install
npm run dev

# Crear build
npm run build

# Ejecutar linter
npm run lint
```
