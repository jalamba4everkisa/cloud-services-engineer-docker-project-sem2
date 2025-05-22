FROM node:16-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
RUN npm run build

FROM nginx:stable-alpine
RUN addgroup -g 1000 nginxgroup && \
    adduser -D -S -h /var/cache/nginx -s /sbin/nologin -G nginxgroup -u 1000 nginxuser && \
    chown -R nginxuser:nginxgroup /var/cache/nginx && \
    chown -R nginxuser:nginxgroup /var/log/nginx && \
    chown -R nginxuser:nginxgroup /etc/nginx/conf.d
RUN touch /var/run/nginx.pid && \
    chown -R nginxuser:nginxgroup /var/run/nginx.pid && \
    chown -R nginxuser:nginxgroup /usr/share/nginx/html
COPY nginx.conf /etc/nginx/conf.d/default.conf
COPY --from=builder --chown=nginx:nginx /app/dist /usr/share/nginx/html/momo-store
EXPOSE 80
USER nginxuser
CMD ["nginx","-g","daemon off;"]