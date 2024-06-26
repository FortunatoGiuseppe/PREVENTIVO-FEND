# Stage 1: Build dell'applicazione Angular
FROM node:14 AS build

# Imposta la directory di lavoro all'interno del container
WORKDIR /app

# Copia il file package.json e package-lock.json per installare le dipendenze
COPY PREVENTIVO-FEND/angular/package*.json ./

# Installa le dipendenze del frontend
RUN npm install

# Copia il resto del codice sorgente del frontend
COPY PREVENTIVO-FEND/angular .

# Compila il frontend
RUN npm run build

# Stage 2: Serve il frontend con Nginx
FROM nginx

# Copia i file statici del frontend compilato
COPY --from=build /app/dist/angular /usr/share/nginx/html

# Copia la configurazione personalizzata di Nginx
COPY PREVENTIVO-FEND/angular/nginx.conf /etc/nginx/conf.d/default.conf

# Esponi la porta su cui Nginx ascolta
EXPOSE 80

# Comando per avviare Nginx all'interno del container
CMD ["nginx", "-g", "daemon off;"]

USER 10001