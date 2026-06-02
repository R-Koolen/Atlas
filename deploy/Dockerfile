# Statische site, geserveerd door Caddy.
# Caddy luistert intern op :8080 (geen privileged poort) zodat de container
# zonder extra capabilities en als non-root kan draaien.
#
# Build-context = repo-root (zie workflow: context "." + file "deploy/Dockerfile").
# COPY-paden zijn daarom relatief aan de repo-root, niet aan deze map.

# 1) Prep-stage (wegwerp): verzamel de site-inhoud en gooi infra eruit.
#    Wat hier wordt verwijderd, belandt NIET in de uiteindelijke image — deze
#    hele stage wordt weggegooid. Dus geen "verwijderde" bestanden in image-lagen.
FROM caddy:2-alpine AS prep
WORKDIR /site
COPY . .
# deploy/ zit in de context (nodig voor de Caddyfile hieronder) maar mag niet
# geserveerd worden. .git/.github/notebooks vallen al af via Dockerfile.dockerignore.
RUN rm -rf deploy

# 2) Final image: alleen Caddy + de schone webroot.
FROM caddy:2-alpine
COPY deploy/Caddyfile /etc/caddy/Caddyfile
COPY --from=prep /site /srv

# Healthcheck zodat de orchestrator weet of de container gezond is
HEALTHCHECK --interval=30s --timeout=3s --start-period=5s --retries=3 \
  CMD wget -q --spider http://127.0.0.1:8080/ || exit 1

EXPOSE 8080
