# Stage 1: Build dell'applicazione Angular
FROM node:18 AS build
WORKDIR /app

# Debug: Mostra il contenuto della directory corrente
RUN ls -la

# Copia package.json e package-lock.json (se presente) dalla directory 'angular'
COPY angular/package*.json ./

# Debug: Verifica che i file package*.json siano stati copiati
RUN ls -la

# Usa un argomento di build per invalidare la cache
ARG CACHEBUST=1

# Installa le dipendenze
RUN npm install

# Copia il resto dei file del progetto dalla directory 'angular'
COPY angular/. .

# Debug: Mostra tutti i file copiati
RUN ls -la

# Esegui il build
RUN npm run build

# Debug: Verifica il contenuto della directory dist
RUN ls -la dist

# Stage 2: Serve the frontend with Nginx
FROM nginx:alpine

# Remove default Nginx configuration
RUN rm -rf /usr/share/nginx/html/*

# Copy built Angular files
COPY --from=build /app/dist/angular /usr/share/nginx/html

# Copy Nginx configuration
COPY nginx.conf /etc/nginx/nginx.conf

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
USER 10001
