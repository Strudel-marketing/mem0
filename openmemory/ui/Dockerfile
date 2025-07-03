# שלב 1: Build
FROM node:18-alpine AS builder

WORKDIR /app

# התקנת תלות
COPY package.json pnpm-lock.yaml ./
RUN corepack enable && corepack prepare pnpm@10.5.2 --activate && pnpm install

# העתקת שאר הקבצים ובנייה
COPY . .
RUN pnpm build

# שלב 2: הרצת פרודקשן
FROM node:18-alpine

WORKDIR /app

# התקנת pnpm להרצה
RUN corepack enable && corepack prepare pnpm@10.5.2 --activate

# העתקת תוצרים מוכנים בלבד
COPY --from=builder /app/.next .next
COPY --from=builder /app/public public
COPY --from=builder /app/package.json ./
COPY --from=builder /app/node_modules node_modules

EXPOSE 3000
ENV PORT=3000
ENV HOSTNAME="0.0.0.0"

CMD ["pnpm", "start"]
