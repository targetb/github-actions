FROM alpine:3

# Set up APK repositories and upgrade
RUN apk add --no-cache --repository=http://dl-cdn.alpinelinux.org/alpine/edge/main git \
    && apk -U upgrade

# Install required tools
RUN apk add --no-cache git docker curl

# renovate: datasource=github-releases depName=mikefarah/yq
ENV YQ_VERSION="v4.25.2"
RUN curl -sSLo /usr/bin/yq \
    "https://github.com/mikefarah/yq/releases/download/${YQ_VERSION}/yq_linux_amd64" \
    && chmod +x /usr/bin/yq

# renovate: datasource=github-releases depName=stedolan/jq extractVersion=^jq-(?<version>.*)$
ENV JQ_VERSION="1.6"
RUN curl -sSLo /usr/bin/jq \
    "https://github.com/stedolan/jq/releases/download/jq-${JQ_VERSION}/jq-linux64" \
		&& chmod +x /usr/bin/jq

ARG account=git
RUN addgroup -S ${account} \
    && adduser -S ${account} -G ${account}
USER ${account}:${account}

WORKDIR /
COPY productmanifest.sh .

# Does not substitute file in quotes...
ENTRYPOINT ["/productmanifest.sh"]
