# Stage 1: Build dell'applicazione Angular
FROM node:14 AS build
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
RUN npm run build

# Stage 2: Serve il frontend con Nginx
FROM nginx
COPY --from=build /app/nginx.conf /etc/nginx/conf.d/default.conf
COPY --from=build /app/dist/angular /usr/share/nginx/html
RUN ls -la
RUN echo ' \
server { \
    listen 80; \
    location / { \
        root /usr/share/nginx/html; \
        try_files $uri $uri/ /index.html; \
    } \
}' > /etc/nginx/conf.d/default.conf
RUN ls -la
RUN pwd
RUN echo "Content of current directory:"
RUN find .
COPY ./nginx.conf /etc/nginx/conf.d/default.conf
COPY /PREVENTIVO-FEND/angular/nginx.conf /etc/nginx/conf.d/default.conf
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
USER 10001