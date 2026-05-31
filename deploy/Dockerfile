# Statische site, geserveerd door Caddy.
# Caddy luistert intern op :8080 (geen privileged poort) zodat de container
# zonder extra capabilities en als non-root kan draaien.
#
# Build-context = repo-root (zie workflow: context "." + file "deploy/Dockerfile").
# COPY-paden zijn daarom relatief aan de repo-root, niet aan deze map.
FROM caddy:2-alpine

# Configuratie + site-inhoud. Wat NIET geserveerd mag worden (git, infra,
# notebooks) staat in deploy/Dockerfile.dockerignore.
COPY deploy/Caddyfile /etc/caddy/Caddyfile
COPY . /srv

# Healthcheck zodat de orchestrator weet of de container gezond is
HEALTHCHECK --interval=30s --timeout=3s --start-period=5s --retries=3 \
  CMD wget -q --spider http://127.0.0.1:8080/ || exit 1

EXPOSE 8080
