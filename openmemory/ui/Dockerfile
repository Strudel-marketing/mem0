# שלב 1: בנייה
FROM node:18-alpine AS builder

WORKDIR /app

COPY package*.json ./
RUN npm install

COPY . .
RUN npm run build

# שלב 2: הרצה
FROM node:18-alpine

WORKDIR /app

COPY --from=builder /app ./

ENV PORT=3000
EXPOSE 3000

CMD ["npm", "start"]
