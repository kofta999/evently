FROM oven/bun:1.3 AS build
WORKDIR /app
COPY package.json bun.lock ./
RUN bun install
COPY . ./
RUN bun run build

FROM oven/bun:1.3-alpine AS production
WORKDIR /app
COPY --from=build /app/dist ./dist
COPY --from=build /app/package.json ./
COPY --from=build /app/node_modules ./node_modules

EXPOSE 3000
CMD ["bun", "dist/src/index.js"]
