# syntax=docker/dockerfile:1

FROM debian:stable-slim

RUN apt update && apt install -y curl ca-certificates moreutils jq prips --no-install-recommends && apt clean

COPY entrypoint /usr/bin/entrypoint
RUN chmod +x /usr/bin/entrypoint

ARG ASPIA_REPO=https://github.com/dchapyshev/aspia
ARG ASPIA_VERSION
ENV ASPIA_PACKAGE=aspia-router

RUN curl -sSL ${ASPIA_REPO}/releases/download/v${ASPIA_VERSION}/${ASPIA_PACKAGE}-${ASPIA_VERSION}-x86_64.deb -o /tmp/${ASPIA_PACKAGE}-${ASPIA_VERSION}-x86_64.deb && \
    dpkg -i /tmp/${ASPIA_PACKAGE}-${ASPIA_VERSION}-x86_64.deb && rm /tmp/${ASPIA_PACKAGE}-${ASPIA_VERSION}-x86_64.deb

EXPOSE 8060/tcp
ENTRYPOINT [ "/usr/bin/entrypoint" ]
