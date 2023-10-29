FROM node:alpine3.18 as ts-compiler
WORKDIR /app
LABEL author="Tudor Seserman"
COPY package*.json ./
COPY tsconfig*.json ./
RUN npm i
COPY . .
EXPOSE 3000
CMD ["npm", "run", "dev"]
