# backend/Dockerfile
FROM node:18-alpine

WORKDIR /usr/src/app

COPY package*.json ./
RUN npm install --only=production

COPY . .

# Environment variable for Mongo connection string (injected via docker-compose)
ENV PORT=3000

EXPOSE 3000

CMD ["npm", "start"]
# or ["node", "server.js"] or whatever your README says
