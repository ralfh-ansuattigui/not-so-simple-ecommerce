FROM node:20-alpine3.22 AS build
WORKDIR /nsse-frontend
COPY package*.json ./
RUN npm ci --silent --only=production
COPY . .
RUN npm run build

FROM nginx:1.25-alpine
COPY --from=build /nsse-frontend/dist /usr/share/nginx/html
RUN rm /etc/nginx/conf.d/default.conf
COPY nginx/nginx.conf /etc/nginx/conf.d/nginx.conf
EXPOSE 80
ENTRYPOINT ["nginx", "-g", "daemon off;"]