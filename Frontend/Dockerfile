# Build stage
FROM node:22-alpine3.20 AS build
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
RUN npm run build

# Production stage
FROM nginx:1.27.3-alpine3.20

# Create a non-root user and group
RUN addgroup -S appgroup && adduser -S appuser -G appgroup

# Copy Nginx configuration
COPY ./nginx.conf /etc/nginx/conf.d/default.conf

# Remove default nginx html files
RUN rm -rf /usr/share/nginx/html/*

# Copy built files from the build stage
COPY --from=build /app/dist /usr/share/nginx/html

# Change ownership of files to the non-root user
RUN chown -R appuser:appgroup /usr/share/nginx /var/cache/nginx /var/log/nginx

# Switch to non-root user
USER appuser

# Expose port 8080 (since ports below 1024 require root privileges)
EXPOSE 8080

# Replace env.js with environment variable values and run nginx
CMD ["/bin/sh",  "-c",  "envsubst < /usr/share/nginx/html/env.template.js > /usr/share/nginx/html/env.js && exec nginx -g 'daemon off;'"]
