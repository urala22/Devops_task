# Stage 1: Build Angular app
FROM node:18-alpine AS build

WORKDIR /usr/src/app
COPY package*.json ./
RUN npm install

COPY . .
# Adjust script name if needed (usually "build": "ng build --configuration production")
RUN npm run build -- --configuration production

# Stage 2: Serve with nginx
FROM nginx:alpine

# Copy built Angular app to nginx html folder
COPY --from=build /usr/src/app/dist/ /usr/share/nginx/html

# Default nginx serves on port 80
EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
