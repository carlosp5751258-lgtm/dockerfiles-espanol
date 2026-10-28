# syntax=docker/dockerfile:1
# =====================================================================
# Dockerfile para Go con imagen final distroless (sin shell, sin
# gestor de paquetes, usuario no root). Resultado típico: 10-25 MB.
#
#   docker build -t mi-servicio .
#   docker run --rm -p 8080:8080 mi-servicio
#
# Multi-arquitectura (compila de forma cruzada, sin emulación lenta):
#   docker buildx build --platform linux/amd64,linux/arm64 -t mi-servicio .
#
# QUE CAMBIAR:
#   - GO_VERSION: igual o mayor que la directiva "go" de tu go.mod.
#   - PAQUETE: ruta del paquete main (ej. ./cmd/api). "." = raíz.
#   - EXPOSE: el puerto donde escucha tu servicio.
# =====================================================================

ARG GO_VERSION=1.24

# --platform=$BUILDPLATFORM: el compilador corre en la arquitectura de
# tu máquina y genera binarios para la arquitectura destino.
FROM --platform=$BUILDPLATFORM golang:${GO_VERSION}-alpine AS build
WORKDIR /src

# Dependencias primero (capa reutilizable mientras go.mod/go.sum no cambien)
COPY go.mod go.sum ./
RUN --mount=type=cache,target=/go/pkg/mod \
    go mod download

COPY . .

ARG TARGETOS
ARG TARGETARCH
ARG PAQUETE=.
# CGO_ENABLED=0: binario estático, no depende de libc.
# -trimpath: quita rutas locales del binario (builds reproducibles).
# -ldflags "-s -w": elimina símbolos de depuración (binario más chico).
RUN --mount=type=cache,target=/go/pkg/mod \
    --mount=type=cache,target=/root/.cache/go-build \
    CGO_ENABLED=0 GOOS=${TARGETOS} GOARCH=${TARGETARCH} \
    go build -trimpath -ldflags="-s -w" -o /out/app ${PAQUETE}

# ---------------------------------------------------------------------
# Runtime: distroless "static" incluye certificados CA, zona horaria y
# un usuario "nonroot" (uid 65532). Nada más.
# ---------------------------------------------------------------------
FROM gcr.io/distroless/static-debian12:nonroot
COPY --from=build /out/app /app
USER nonroot:nonroot
EXPOSE 8080

# Distroless no tiene shell ni curl, así que no hay HEALTHCHECK aquí.
# Opciones: healthcheck en docker-compose/Kubernetes contra tu endpoint,
# o que tu binario acepte un flag (ej. "/app -healthcheck") y usar:
#   HEALTHCHECK CMD ["/app", "-healthcheck"]

ENTRYPOINT ["/app"]
