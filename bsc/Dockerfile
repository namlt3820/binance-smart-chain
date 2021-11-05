FROM debian:bullseye-slim


RUN apt-get update -y \
  && apt-get install -y curl jq unzip nano net-tools \
  && apt-get clean \
  && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

RUN curl --silent "https://api.github.com/repos/binance-chain/bsc/releases/tags/v1.1.4" | jq -c '.assets[] | select( .browser_download_url | contains("testnet.zip") or contains("geth_linux")) | .browser_download_url' | xargs -n1 curl -LOJ && \
    unzip testnet.zip -d / && \
    sed -i '/Node\.LogConfig/,/^$/d' /config.toml && \
    sed -i 's/HTTPHost = "127.0.0.1"/HTTPHost = "0.0.0.0"/g' /config.toml && \
    mv geth_linux /usr/local/bin/bsc && \
    chmod +x /usr/local/bin/bsc


ENV BSC_DATADIR=/root/.ethereum

COPY docker-entrypoint.sh /entrypoint.sh

#VOLUME ["$BSC_DATADIR"]

EXPOSE 8575 8576 8577 30311 30311/udp

ENTRYPOINT ["/entrypoint.sh"]

CMD ["bsc", "-ws", "-ws.addr", "0.0.0.0", "-ws.origins", "*", "-syncmode", "fast", "-nousb", "-cache", "8192", "-maxpeers", "1000"]