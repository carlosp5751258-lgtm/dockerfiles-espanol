# Dockerfiles optimizados en español (gratis)

🇬🇧 [Read in English](README.en.md)

Dockerfiles multi-stage, sin root y comentados en español: imágenes pequeñas y builds rápidos.

**Gratis y de código abierto (MIT).** Úsalo en tus proyectos y en los de tus clientes.

## Qué incluye

- `go/`: Dockerfile distroless multi-arquitectura
- `node-express/`: alpine, tini y healthcheck sin curl
- `.dockerignore` para cada uno

## Instalación

```bash
git clone https://github.com/carlosp5751258-lgtm/dockerfiles-espanol.git
cp dockerfiles-espanol/node-express/Dockerfile dockerfiles-espanol/node-express/.dockerignore .   # o go/
docker build -t mi-app .
```



## ¿Quieres la versión completa?

Esta es la versión gratis y funciona sola. La **versión Pro** añade:

- Next.js con salida standalone y Python/FastAPI (uv y pip)
- Go con imagen `scratch`
- docker-compose con app + PostgreSQL + Redis y healthchecks

👉 [Ver la versión Pro](https://claudecodees.gumroad.com/l/dockerfiles-optimizados) · paga lo que quieras desde 2 USD. Comprarla también ayuda a que sigamos publicando recursos gratis.

## Licencia

MIT: puedes usarlo, modificarlo y compartirlo, también en proyectos comerciales. Ver [LICENSE](LICENSE).

---
Parte de **[Ctrl+IA](https://ctrlia.dev/)**: recursos en español para programar con IA. Hecho por Carlos P.
