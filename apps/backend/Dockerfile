FROM oven/bun:1.2 AS build
WORKDIR /app
COPY package.json bun.lockb ./
COPY apps/backend/package.json apps/backend/
COPY apps/frontend/package.json apps/frontend/
RUN bun install --frozen-lockfile
COPY apps/backend/ apps/backend/
RUN cd apps/backend && bun run build

FROM node:20-alpine AS production
WORKDIR /app
COPY --from=build /app/apps/backend/dist ./dist
COPY --from=build /app/apps/backend/package.json ./
COPY --from=build /app/node_modules ./node_modules

EXPOSE 9999
CMD ["node", "dist/src/index.js"]