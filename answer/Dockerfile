ARG VERSION

FROM apache/answer:${VERSION} as answer-builder

FROM golang:1.22-alpine3.18 AS golang-builder

COPY --from=answer-builder /usr/bin/answer /usr/bin/answer

RUN apk --no-cache add \
    build-base git bash nodejs npm go && \
    npm install -g pnpm@9.7.0

RUN answer build \
    --with github.com/apache/answer-plugins/captcha-basic \
    --with github.com/apache/answer-plugins/connector-basic \
    --with github.com/apache/answer-plugins/editor-chart \
    --with github.com/apache/answer-plugins/editor-formula \
    --with github.com/apache/answer-plugins/embed-basic \
    --with github.com/apache/answer-plugins/render-markdown-codehighlight \
    --with github.com/apache/answer-plugins/reviewer-akismet \
    --with github.com/apache/answer-plugins/reviewer-basic \
    --with github.com/apache/answer-plugins/search-algolia \
    --with github.com/apache/answer-plugins/search-elasticsearch \
    --with github.com/apache/answer-plugins/search-meilisearch \
    --with github.com/apache/answer-plugins/storage-s3 \
    --output /usr/bin/new_answer

FROM alpine:3.18

ARG TIMEZONE="Asia/Shanghai"

RUN apk update \
    && apk --no-cache add \
    bash \
    ca-certificates \
    curl \
    dumb-init \
    gettext \
    openssh \
    sqlite \
    gnupg \
    tzdata \
    && ln -sf /usr/share/zoneinfo/${TIMEZONE} /etc/localtime \
    && echo "${TIMEZONE}" > /etc/timezone

COPY --from=golang-builder /usr/bin/new_answer /usr/bin/answer
COPY --from=answer-builder /data /data
COPY --from=answer-builder /entrypoint.sh /entrypoint.sh
RUN chmod 755 /entrypoint.sh

VOLUME /data
EXPOSE 80
ENTRYPOINT ["/entrypoint.sh"]