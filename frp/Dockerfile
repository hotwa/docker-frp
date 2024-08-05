FROM alpine:latest
LABEL MAINTAINER=pylyzeng@gmail.com

WORKDIR /

# Define build argument and environment variable for FRP version
ARG FRP_TYPE=frps
ENV FRP_TYPE=${FRP_TYPE}

# Install dependencies and set timezone
RUN set -x && \
    apk add -U tzdata curl && \
    cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime && \
    apk del tzdata && \
    curl -o version.latest https://api.github.com/repos/fatedier/frp/releases/latest && \
    FRP_VERSION=`cat version.latest | grep -E 'tag_name\": \"v[0-9]+\.[0-9]+\.[0-9]+' -o |head -n 1| tr -d 'tag_name\": \"v'` && \
    curl -OL https://github.com/fatedier/frp/releases/download/v${FRP_VERSION}/frp_${FRP_VERSION}_linux_amd64.tar.gz && \
    mkdir -p /usr/local/bin/frp && \
    tar xzf frp_${FRP_VERSION}_linux_amd64.tar.gz -C /usr/local/bin/frp --strip-components=1 && \
    mv /usr/local/bin/frp/${FRP_TYPE} /usr/local/bin/ && \
    rm -rf version.latest && \
    rm -rf frp_${FRP_VERSION}_linux_amd64.tar.gz && \
    rm -rf /usr/local/bin/frp

# Expose necessary ports
EXPOSE 7000 7001 7500 8080 8443

# Set entrypoint
ENTRYPOINT ["/usr/local/bin/${FRP_TYPE}","-c","/etc/frp/frpc.toml"]
