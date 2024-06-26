# Utilizza l'immagine di base Node.js con la versione desiderata
FROM node:14.17.0-alpine as builder

# Imposta la directory di lavoro all'interno del container
WORKDIR /app

# Copia i file del progetto nell'immagine del container
COPY . .

# Installa le dipendenze del progetto
RUN npm install

# Build dell'applicazione Angular per la produzione
RUN npm run build -- --prod

# Seconda fase per creare un'immagine leggera con solo l'output di build
FROM nginx:1.21.0-alpine

# Copia i file di build dall'immagine del builder all'immagine nginx
COPY --from=builder /app/dist /usr/share/nginx/html

# Copia la configurazione Nginx personalizzata per Angular
COPY nginx-custom.conf /etc/nginx/conf.d/default.conf

# Esponi la porta 80 per il traffico HTTP
EXPOSE 80

# Comando di avvio per avviare Nginx quando il contenitore viene eseguito
CMD ["nginx", "-g", "daemon off;"]

USER 10001