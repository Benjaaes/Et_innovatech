# Etapa 1: Build
FROM node:20-alpine AS build
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
RUN npm run build

# Etapa 2: Serve
FROM nginx:1.27-alpine
COPY --from=build /app/dist /usr/share/nginx/html
EXPOSE 80
CMD sh -c "echo \"window.APP_CONFIG = { VENTAS_API_URL: '${VENTAS_API_URL:-http://localhost:8080/api/v1/ventas}', DESPACHOS_API_URL: '${DESPACHOS_API_URL:-http://localhost:8081/api/v1/despachos}' }\" > /usr/share/nginx/html/env-config.js && nginx -g 'daemon off;'"
