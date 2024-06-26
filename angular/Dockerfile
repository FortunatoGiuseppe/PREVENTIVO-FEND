# Usa l'immagine Node.js ufficiale come immagine genitore
FROM node:14

# Imposta la directory di lavoro all'interno del container
WORKDIR /app

# Copia il file package.json e package-lock.json per installare le dipendenze
COPY package*.json ./

# Installa le dipendenze del frontend
RUN npm install

# Copia il resto del codice sorgente del frontend
COPY . .

# Compila il frontend
RUN npm run build

# Stage finale: Usa un'immagine di Nginx standard per il server HTTP
FROM nginx

# Copia i file statici del frontend dal build stage al server Nginx
COPY --from=0 /app/build /usr/share/nginx/html

# Copia la configurazione personalizzata di Nginx
COPY nginx.conf /etc/nginx/conf.d/default.conf

# Esponi la porta su cui Nginx ascolta
EXPOSE 80

# Comando per avviare Nginx all'interno del container
CMD ["nginx", "-g", "daemon off;"]

USER 10001