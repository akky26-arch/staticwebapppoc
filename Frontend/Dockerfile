FROM node:20 as build
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
RUN npm run build
 
FROM nginx:latest
COPY ./nginx.conf /etc/nginx/conf.d/default.conf
RUN rm -rf /usr/share/nginx/html/*
COPY --from=build /app/dist /usr/share/nginx/html
COPY ./env.js /usr/share/nginx/html
EXPOSE 80
# When the container starts, replace the env.js with values from environment variables
CMD ["nginx", "-g", "daemon off;"]