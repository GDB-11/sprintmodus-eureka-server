# sprintmodus-eureka-server

## Docker

`Dockerfile` (multi-stage: Maven build on `eclipse-temurin:26-jdk` -> `eclipse-temurin:26-jre`, layered jar, non-root uid 10001, exec-form
entrypoint, `HEALTHCHECK` on `/actuator/health/liveness`). Nothing secret or environment-specific is baked in: every setting
comes from the environment at run time (see `application.properties`); orchestration lives in `sprintmodus-infrastructure`.

```bash
docker build -t ghcr.io/gdb-11/sprintmodus-eureka-server:dev .
```

CI (`.github/workflows/ci.yml`) runs the tests, then builds and pushes `ghcr.io/gdb-11/sprintmodus-eureka-server` tagged `sha-<commit>` on
`main` and `<version>` on a `v<version>` tag, never `latest`. The tests need Docker themselves, so the image build skips them.
