FROM node:22.9.0

RUN mkdir -p /app/backend

COPY package.json /app/backend/

WORKDIR /app/backend

RUN npm install -g npm@11.4.2

COPY . /app/backend

EXPOSE 5000 

CMD ["node", "server.js"]