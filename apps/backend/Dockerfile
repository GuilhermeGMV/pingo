FROM node:20-alpine AS pruner

WORKDIR /app

RUN npm install -g turbo@latest

COPY . .

RUN turbo prune @pingo/backend --docker


FROM node:20-alpine AS builder

WORKDIR /app

RUN npm install -g pnpm

COPY --from=pruner /app/out/json/ .
COPY --from=pruner /app/out/pnpm-lock.yaml ./pnpm-lock.yaml
COPY --from=pruner /app/out/pnpm-workspace.yaml ./pnpm-workspace.yaml

RUN pnpm install --frozen-lockfile

COPY --from=pruner /app/out/full/ .
COPY --from=pruner /app/tsconfig.json ./tsconfig.json

RUN pnpm --filter @pingo/backend exec prisma generate

RUN pnpm turbo build --filter=@pingo/backend

RUN pnpm --filter @pingo/backend deploy --prod --legacy /app/standalone

RUN apk add --no-cache curl


FROM node:20-alpine AS runner

WORKDIR /app

RUN addgroup --system --gid 1001 nodejs
RUN adduser  --system --uid 1001 appuser

COPY --from=builder --chown=appuser:nodejs /app/standalone/node_modules  ./node_modules
COPY --from=builder --chown=appuser:nodejs /app/standalone/package.json  ./package.json


COPY --from=builder --chown=appuser:nodejs /app/apps/backend/dist        ./dist
COPY --from=builder --chown=appuser:nodejs /app/apps/backend/prisma      ./prisma
COPY --from=builder --chown=appuser:nodejs /app/apps/backend/prisma.config.ts ./prisma.config.ts



USER appuser

EXPOSE 3001

CMD ["sh", "-c", "./node_modules/.bin/prisma migrate deploy && node dist/server.js"]