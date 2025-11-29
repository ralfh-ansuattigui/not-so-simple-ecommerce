FROM node:25.3.0-alpine AS build
WORKDIR /nsse-frontend
COPY package*.json ./
RUN npm ci --silent --only=production
COPY . .
RUN npm run build

FROM nginx:1.29.4-alpine AS runtime
RUN apk update && apk upgrade --no-cache
COPY --from=build /nsse-frontend/dist /usr/share/nginx/html
RUN rm /etc/nginx/conf.d/default.conf
COPY nginx/nginx.conf /etc/nginx/conf.d/nginx.conf
EXPOSE 80
ENTRYPOINT ["nginx", "-g", "daemon off;"]