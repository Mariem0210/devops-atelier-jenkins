FROM node:22-alpine AS build
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
# les appels API passent par nginx (/api/) au lieu de localhost:8080
RUN find src -name '*.ts' -exec sed -i 's#http://localhost:8080/#/api/#g' {} +
RUN npm run build

FROM nginx:alpine
COPY nginx.conf /etc/nginx/conf.d/default.conf
COPY --from=build /app/dist/*/browser /usr/share/nginx/html
EXPOSE 80
