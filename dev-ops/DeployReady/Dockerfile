FROM node:20-alpine

WORKDIR /app

COPY app/package*.json ./

RUN npm install

COPY app .

RUN chown -R node:node /app

USER node

EXPOSE 3000

CMD ["npm", "start"]