# 短剧库 红果版后端 · Docker（仅红果）
# docker build -t juku:hongguo .
FROM golang:1.26-bookworm AS build
WORKDIR /src
COPY backend/go.mod ./
COPY backend/main.go ./
COPY backend/internal ./internal
ARG CGO_ENABLED=0
RUN CGO_ENABLED=${CGO_ENABLED} go build -trimpath \
      -ldflags="-s -w -X juku/internal/app.forceHongguoOnly=1" \
      -o /out/juku .

FROM debian:bookworm-slim
ENV TZ=Asia/Shanghai \
    JUKU_HONGGUO_ONLY=1
RUN apt-get update && apt-get install -y --no-install-recommends \
      ca-certificates ffmpeg tzdata tini curl \
    && rm -rf /var/lib/apt/lists/* \
    && groupadd --gid 1000 juku && useradd --uid 1000 --gid 1000 --no-create-home juku \
    && mkdir -p /data /downloads && chown juku:juku /data /downloads
COPY --from=build /out/juku /usr/local/bin/juku
USER 1000:1000
WORKDIR /data
VOLUME ["/data", "/downloads"]
EXPOSE 8998
HEALTHCHECK --interval=30s --timeout=5s --start-period=20s --retries=3 \
  CMD curl --noproxy '*' --fail --silent --output /dev/null http://127.0.0.1:8998/ || exit 1
ENTRYPOINT ["tini", "--"]
CMD ["juku", "-listen", "0.0.0.0:8998", "-open=false", "-data-dir", "/data", "-out", "/downloads"]
