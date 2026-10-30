# Optimized Dockerfiles (free)

🇪🇸 [Leer en español](README.md)

Multi-stage, non-root, commented Dockerfiles: small images and fast builds.

**Free and open source (MIT).** Use it in your projects and your clients' projects. The files are commented in Spanish.

## What's included

- `go/`: multi-architecture distroless Dockerfile
- `node-express/`: alpine, tini and a healthcheck without curl
- A `.dockerignore` for each one

## Installation

```bash
git clone https://github.com/carlosp5751258-lgtm/dockerfiles-espanol.git
cp dockerfiles-espanol/node-express/Dockerfile dockerfiles-espanol/node-express/.dockerignore .   # or go/
docker build -t mi-app .
```



## Want the full version?

This is the free version and it works on its own. The **Pro version** adds:

- Next.js with standalone output and Python/FastAPI (uv and pip)
- Go with a `scratch` image
- docker-compose with app + PostgreSQL + Redis and healthchecks

👉 [See the Pro version](https://claudecodees.gumroad.com/l/dockerfiles-optimizados) · pay what you want from 2 USD. Buying it also helps us keep publishing free resources.

## License

MIT: you can use, modify and share it, including in commercial projects. See [LICENSE](LICENSE).

---
Part of **[Ctrl+IA](https://ctrlia.dev/en/)**: resources for coding with AI. Made by Carlos P.
