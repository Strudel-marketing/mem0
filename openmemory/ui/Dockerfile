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

# DEBUG: הדפס את הקונפיגים
RUN echo "🧾 next.config.mjs:" && cat next.config.mjs || echo "not found"
RUN echo "📁 All files:" && ls -R

# הרץ את הבנייה
RUN pnpm build && find .next/standalone -name "*.js" || echo "❌ not found"

# DEBUG: ודא שהקובץ קיים
RUN echo "📁 Contents of .next/standalone:" && ls -R .next/standalone || echo "❌ Not created"

FROM base AS runner
WORKDIR /app
ENV NODE_ENV=production
ENV PORT=3000
ENV HOSTNAME=0.0.0.0

COPY --from=builder /app/public ./public
COPY --from=builder /app/.next/standalone ./
COPY --from=builder /app/.next/static ./.next/static

EXPOSE 3000
CMD ["node", "index.js"]
