# Docker, Laravel, React, Nginx, MySQL8 - Multiplataforma (Windows/Linux)

Proyecto Full-Stack con Laravel (Backend) y React+Vite (Frontend) usando Docker, optimizado para compatibilidad Windows/Linux.

## 🚀 Características

- **Backend:** Laravel 11 con PHP 8.2
- **Frontend:** React 19 + Vite + TypeScript + Material-UI
- **Base de datos:** MySQL 8.0
- **Servidor web:** Nginx
- **Compatible:** Windows y Linux
- **Hot Reload:** Habilitado para desarrollo

## 📋 Requisitos Previos

- Docker Desktop instalado y ejecutándose
- Git (opcional)

## ⚙️ Configuración del Entorno

### 1. Clonar o descargar el proyecto

```bash
git clone <repository-url>
cd apuesta-digital-docker
```

### 2. Configuración de variables de entorno

El archivo `.env` ya está configurado con valores por defecto:
```bash
MYSQL_DATABASE=apuesta
MYSQL_USER=admin
MYSQL_PASSWORD=admin
MYSQL_ROOT_PASSWORD=admin
```

El archivo `frontend/.env` está configurado para desarrollo:
```bash
VITE_API_BASE_URL=http://server-nginx
```

## 🚀 Ejecución del proyecto

### Opción 1: Scripts automatizados

#### En Windows (PowerShell):
```powershell
.\start-project.ps1
```

#### En Linux/Mac (Bash):
```bash
chmod +x start-project.sh
./start-project.sh
```

### Opción 2: Comandos Docker Compose manuales

```bash
# Limpiar contenedores existentes
docker compose down -v

# Construir y ejecutar todos los servicios
docker compose up --build

# Ejecutar en segundo plano
docker compose up --build -d
```

### Opción 3: Ejecutar servicios individuales

```bash
# Solo el frontend
docker compose up --build frontend

# Solo el backend (PHP + Nginx + MySQL)
docker compose up --build server-nginx php82 mysql8
```

## 🌐 Servicios disponibles

Una vez iniciados los contenedores:

- **Frontend (React):** [http://localhost:5173](http://localhost:5173)
- **Backend (Laravel):** [http://localhost:8081](http://localhost:8081)
- **Base de datos MySQL:** `localhost:3310`

## 🛠️ Comandos útiles

### Ver logs de los servicios
```bash
# Logs de todos los servicios
docker compose logs -f

# Logs de un servicio específico
docker compose logs -f frontend
docker compose logs -f server-nginx
docker compose logs -f php82
docker compose logs -f mysql8
```

### Ejecutar comandos dentro de los contenedores
```bash
# Entrar al contenedor PHP para ejecutar comandos Laravel
docker compose exec php82 bash

# Instalar dependencias de Composer
docker compose exec php82 composer install

# Ejecutar migraciones
docker compose exec php82 php artisan migrate

# Entrar al contenedor frontend
docker compose exec frontend sh

# Instalar nuevos paquetes npm
docker compose exec frontend npm install [paquete]
```

### Base de datos
```bash
# Conectar a MySQL
docker compose exec mysql8 mysql -u admin -p apuesta

# Backup de la base de datos
docker compose exec mysql8 mysqldump -u admin -p apuesta > backup.sql

# Restaurar backup
docker compose exec -T mysql8 mysql -u admin -p apuesta < backup.sql
```

## 🔧 Desarrollo

### Estructura del proyecto
```
apuesta-digital-docker/
├── frontend/                 # Aplicación React
│   ├── src/
│   ├── package.json
│   └── vite.config.ts
├── src/                      # Aplicación Laravel
│   ├── app/
│   ├── routes/
│   └── composer.json
├── docker_stack/             # Configuraciones Docker
│   ├── nginx/
│   ├── php/
│   └── react/
├── docker-compose.yml        # Configuración principal
└── .env                      # Variables de entorno
```

### Hot Reload

- **Frontend:** Vite está configurado con hot reload automático
- **Backend:** Los cambios en PHP se reflejan automáticamente

### Troubleshooting

#### Problema: Frontend no arranca
```bash
# Reconstruir solo el frontend
docker compose build --no-cache frontend
docker compose up frontend
```

#### Problema: Permisos en Windows
```bash
# Limpiar volúmenes
docker compose down -v
docker volume prune -f
docker compose up --build
```

#### Problema: Puerto ocupado
```bash
# Verificar puertos en uso
netstat -an | findstr ":5173"  # Windows
lsof -i :5173                  # Linux/Mac

# Cambiar puertos en docker-compose.yml si es necesario
```

## 🐛 Solución de problemas comunes

### Windows específicos

1. **WSL2 requerido:** Asegúrate de tener WSL2 habilitado
2. **Memoria insuficiente:** Aumenta la memoria asignada a Docker Desktop
3. **Firewall:** Permite Docker Desktop en el firewall de Windows

### Linux específicos

1. **Permisos Docker:** Añade tu usuario al grupo docker
   ```bash
   sudo usermod -aG docker $USER
   ```
2. **Memoria:** Libera memoria si el build falla
   ```bash
   docker system prune -a
   ```

## 📝 Notas de desarrollo

- Los `node_modules` se mantienen en un volumen Docker para evitar problemas de permisos entre Windows/Linux
- Vite está configurado con polling para detectar cambios en sistemas de archivos montados
- Las dependencias de Rollup están optimizadas para múltiples arquitecturas

## 🚫 Detener el proyecto

```bash
# Detener servicios
docker compose down

# Detener y eliminar volúmenes
docker compose down -v

# Limpiar completamente
docker compose down -v --rmi all
```

## Acceso a los servicios

- **Frontend (React + Vite)**: http://localhost:5173
- **Backend (Laravel + Nginx)**: http://localhost:8081
- **Base de datos MySQL**: localhost:3310

## Estructura del proyecto

```
apuesta-digital-docker/
├── docker-compose.yml          # Configuración de servicios Docker
├── .env                        # Variables de entorno
├── start.ps1                   # Script de inicio para Windows
├── start.sh                    # Script de inicio para Linux/Mac
├── docker_stack/               # Configuraciones de Docker
│   ├── nginx/                  # Configuración de Nginx
│   ├── php/                    # Configuración de PHP-FPM
│   ├── react/                  # Dockerfile para React
│   └── mysql/                  # Logs de MySQL
├── frontend/                   # Aplicación React + TypeScript
│   ├── src/                    # Código fuente del frontend
│   ├── package.json            # Dependencias de Node.js
│   ├── vite.config.ts          # Configuración de Vite
│   └── .env                    # Variables de entorno del frontend
└── src/                        # Aplicación Laravel
    ├── app/                    # Código de la aplicación
    ├── config/                 # Configuraciones
    ├── database/               # Migraciones y seeders
    └── routes/                 # Rutas de la API
```

## Solución de problemas comunes

### Problema con permisos en Windows
Si encuentras errores de permisos:
1. Asegúrate de que Docker Desktop esté ejecutándose como administrador
2. Habilita la compartición de unidades en Docker Desktop
3. Usa el script PowerShell `start.ps1`

### Problemas con node_modules
El proyecto usa volúmenes nombrados para `node_modules` para evitar conflictos de permisos entre Windows y Linux.

### Frontend no se actualiza automáticamente
Vite está configurado con polling para detectar cambios en Windows:
```typescript
watch: {
  usePolling: true,
  interval: 1000,
}
```

### Reconstruir contenedores
Si necesitas reconstruir completamente:
```bash
docker-compose down -v
docker system prune -f
docker-compose up --build --force-recreate
```

## Desarrollo

### Comandos útiles

```bash
# Ver logs de un servicio específico
docker-compose logs frontend
docker-compose logs php82

# Ejecutar comandos dentro de un contenedor
docker-compose exec php82 bash
docker-compose exec frontend sh

# Detener servicios
docker-compose down

# Detener servicios y eliminar volúmenes
docker-compose down -v
```

### Hot Reload
El hot reload está habilitado para el frontend. Los cambios en el código se reflejarán automáticamente en el navegador.

## Características implementadas para compatibilidad multiplataforma

1. **Volúmenes separados**: `node_modules` en volumen nombrado para evitar conflictos de permisos
2. **Polling de archivos**: Configurado en Vite para Windows
3. **Scripts de inicio**: Específicos para PowerShell y Bash
4. **Dockerfile optimizado**: Con dependencias para múltiples arquitecturas
5. **Variables de entorno**: Configuración centralizada
6. **Hot Module Replacement**: Configurado para funcionar en contenedores

5. Configure vite.config.js in frontend (if necessary)


```bash
    # Before running npm run dev, configure the vite.config.js file in the root directory of the frontend container. It should look like this
    import { defineConfig } from 'vite';
    import react from '@vitejs/plugin-react';

    export default defineConfig({
        plugins: [react()],
        server: {
            host: '0.0.0.0',
            port: 5173,
            strictPort: true
        },
        base: '/'
    });

```

5. Run docker and build

```bash
    #Executed from the linux console
    docker compose up -d --build
```

## Create Project  laravel 
With composer:

```bash
    #Executed from the linux console
    docker compose run --rm php82 composer create-project laravel/laravel .
```


If access Forbidden

```bash
    #Executed from the linux console
    docker compose run --rm --user root php82 chown -R laravel:laravel /var/www/html

    # Executed from the linux console
    docker compose run --rm php82 /bin/sh

    chmod -R 777 /var/www/html/storage
    chmod -R 777 /var/www/html/bootstrap/cache

```
## Test app

Open the browser and enter http://localhost:8081/ access the laravel project
Open the browser and access http://localhost:5173/ access the react project

## Run command line in container

To enter the php container console

```bash
    #Executed from the linux console
    docker compose run --rm php82 /bin/sh
```

To run npm, composer, artisan

```bash
    #Executed from the linux console
    #Replace "command" by the corresponding command, example docker compose run --rm php82 php artisan list
    docker compose run --rm npm "command"

    docker compose run --rm php82 composer "command"

    docker compose run --rm php82 php artisan "command"
```
Stop containers

```bash
    #Executed from the console
    docker-compose stop

```

Stop and delete containers

```bash
    #Executed from the console
    docker-compose down

```
# Considerations

If you modify port values, verify where it is used and change it otherwise it will not work correctly.

If you already have docker containers, verify not to use the same local ports in order not to have conflicts, in case you want to use the same ones, those containers must be turned off when using this one.
