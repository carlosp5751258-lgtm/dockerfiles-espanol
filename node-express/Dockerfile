# syntax=docker/dockerfile:1
# =====================================================================
# Dockerfile multi-stage para Node.js / Express (JavaScript o TypeScript)
#
#   docker build -t mi-api .
#   docker run --rm -p 3000:3000 mi-api
#
# QUE CAMBIAR:
#   - NODE_VERSION: usa la misma versión mayor que en desarrollo.
#   - El CMD final: la ruta a tu archivo de entrada (ej. dist/index.js
#     si compilas TypeScript, src/server.js si es JavaScript puro).
#   - El puerto (EXPOSE y la ruta /health del HEALTHCHECK).
# REQUISITOS: package-lock.json versionado (npm ci lo necesita).
# =====================================================================

ARG NODE_VERSION=22

# ---------------------------------------------------------------------
# Etapa 1: base común (imagen alpine: ~50 MB frente a ~350 MB de la normal)
# ---------------------------------------------------------------------
FROM node:${NODE_VERSION}-alpine AS base
WORKDIR /app

# ---------------------------------------------------------------------
# Etapa 2: build. Instala TODO (incluye devDependencies para compilar),
# compila si existe el script "build" y luego elimina las devDependencies.
# ---------------------------------------------------------------------
FROM base AS build
# Copiamos primero solo los manifiestos: mientras no cambien, Docker
# reutiliza la capa de "npm ci" aunque cambies tu código.
COPY package.json package-lock.json ./
# La caché de npm se monta solo durante el build (no queda en la imagen).
RUN --mount=type=cache,target=/root/.npm \
    npm ci
COPY . .
RUN npm run build --if-present \
 && npm prune --omit=dev

# ---------------------------------------------------------------------
# Etapa 3: runtime. Solo lo necesario para ejecutar.
# ---------------------------------------------------------------------
FROM base AS runtime
ENV NODE_ENV=production \
    PORT=3000

# tini: init mínimo que reenvía señales (SIGTERM) y recoge procesos
# zombie. Sin él, "docker stop" espera 10 s y mata el proceso a la fuerza.
RUN apk add --no-cache tini

# El usuario "node" (uid 1000) ya existe en la imagen oficial.
COPY --from=build --chown=node:node /app ./
USER node

EXPOSE 3000

# Requiere una ruta /health en tu app que responda 200.
# Usa fetch nativo de Node (18+): no hace falta instalar curl.
HEALTHCHECK --interval=30s --timeout=5s --start-period=10s --retries=3 \
  CMD node -e "fetch('http://127.0.0.1:'+(process.env.PORT||3000)+'/health').then(r=>process.exit(r.ok?0:1)).catch(()=>process.exit(1))"

ENTRYPOINT ["/sbin/tini", "--"]
# Forma "exec" (JSON): node recibe las señales directamente.
# Evita "npm start" en producción: npm no reenvía bien SIGTERM.
CMD ["node", "src/index.js"]
