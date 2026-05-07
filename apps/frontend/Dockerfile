FROM oven/bun:1.3 AS deps
WORKDIR /repo

# Copy workspace root manifests first (for better layer caching)
COPY package.json bun.lock ./

# Copy only package manifests for workspaces needed for dependency graph resolution
COPY apps/frontend/package.json apps/frontend/package.json
COPY apps/backend/package.json apps/backend/package.json

RUN bun install --frozen-lockfile

FROM deps AS build
WORKDIR /repo

# Copy full sources after deps installed
COPY apps/frontend apps/frontend
COPY apps/backend apps/backend

# Build frontend
WORKDIR /repo/apps/frontend
RUN bun run build

FROM nginx:alpine AS runtime
COPY --from=build /repo/apps/frontend/dist /usr/share/nginx/html
COPY apps/frontend/nginx.conf /etc/nginx/conf.d/default.conf
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
