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

# Stage 2: Serve il frontend con Nginx
FROM nginx:alpine

# Rimuovi la configurazione predefinita di Nginx
RUN rm -rf /usr/share/nginx/html/*

# Copia i file buildate da Angular
COPY --from=build /app/dist/angular /usr/share/nginx/html

# Copia il file di configurazione di Nginx
COPY nginx.conf /etc/nginx/conf.d/default.conf

# Debug: Verifica il contenuto della directory di Nginx
RUN ls -la /usr/share/nginx/html

# Debug: Mostra il contenuto del file di configurazione di Nginx
RUN cat /etc/nginx/conf.d/default.conf

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
USER 10001
