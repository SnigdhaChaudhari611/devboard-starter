# Stage 1 — build the app (build tools stay in this stage only)
FROM dhi.io/node:26-dev AS build

WORKDIR /app

COPY package*.json ./
RUN npm ci --legacy-peer-deps

COPY . .
RUN npm run build

# Stage 2 — serve the static build with nginx (no Node, no node_modules)
FROM nginxinc/nginx-unprivileged:stable-alpine

# Replace the default site config with ours (serves dist + proxies /api)
COPY nginx.conf /etc/nginx/conf.d/default.conf

# Only the built files ship to production
COPY --from=build /app/dist /usr/share/nginx/html

EXPOSE 4173