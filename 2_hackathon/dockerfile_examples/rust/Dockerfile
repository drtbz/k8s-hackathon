# Builder
FROM rust:1.58.0-bullseye as build

WORKDIR /app
COPY . .

# build server package
RUN cargo build -p <package-name> --release

# The final image
FROM debian:bullseye-slim
LABEL org.opencontainers.image.source https://github.com/<your-username>/<your-repo>

RUN mkdir -p /usr/local/bin
COPY --from=build app/target/release/<package-name> /usr/local/bin/

ENTRYPOINT ["<package-name>"]
