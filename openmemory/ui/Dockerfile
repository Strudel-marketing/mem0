FROM node:18-alpine AS base
RUN apk add --no-cache libc6-compat curl && \
    corepack enable && \
    corepack prepare pnpm@latest --activate
WORKDIR /app

FROM base AS deps
COPY package.json pnpm-lock.yaml ./
RUN pnpm install --frozen-lockfile

FROM base AS builder
WORKDIR /app
COPY --from=deps /app/node_modules ./node_modules
COPY . .

# וידוא שהקובץ הנכון מועתק
RUN echo "🧾 next.config.mjs:" && cat next.config.mjs || echo "❌ not found"
RUN pnpm build

# הדפסת כל הקבצים שנבנו
RUN echo "📂 ALL FILES:" && find .next -type f

FROM base AS runner
WORKDIR /app
ENV NODE_ENV=production
ENV PORT=3000
ENV HOSTNAME=0.0.0.0

COPY --from=builder /app/public ./public
COPY --from=builder /app/.next/standalone ./
COPY --from=builder /app/.next/static ./.next/static

# ננסה להריץ main.js אם הוא קיים
EXPOSE 3000
CMD ["node", "main.js"]
