# Usa la imagen base oficial de Node
FROM node:20-alpine

# Instalar dependencias del sistema para compatibilidad
RUN apk add --no-cache python3 make g++

# Definir el directorio de trabajo
WORKDIR /app

# Copiar los archivos package.json y package-lock.json
COPY package*.json ./

# Limpiar cache de npm
RUN npm cache clean --force

# Instalar las dependencias de npm
RUN npm install --legacy-peer-deps

# Instalar dependencias específicas para arquitecturas multiplataforma
RUN npm install @rollup/rollup-linux-x64-musl --save-optional || true
RUN npm install @rollup/rollup-linux-x64-gnu --save-optional || true
RUN npm install @rollup/rollup-win32-x64-msvc --save-optional || true

# Copiar todo el contenido de la carpeta frontend
COPY . .

# Cambiar permisos para compatibilidad Windows/Linux
RUN chmod -R 755 /app

# Exponer el puerto 5173 (que es el puerto por defecto de Vite)
EXPOSE 5173

# Comando por defecto para ejecutar la app con Vite
CMD ["npm", "run", "dev", "--", "--host", "0.0.0.0", "--port", "5173"]
