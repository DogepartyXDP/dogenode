FROM counterparty/base

MAINTAINER Dogeparty Developers <dev@dogeparty.net>

RUN apt-get update && apt-get install -y pv curl

# install dogecoin core
ENV DOGECOIN_VER="1.14.9"
ENV DOGECOIN_FOLDER_VER="1.14.9"
ENV DOGECOIN_SHASUM="4f227117b411a7c98622c970986e27bcfc3f547a72bef65e7d9e82989175d4f8"

WORKDIR /tmp

RUN wget -O dogecoin-${DOGECOIN_VER}-x86_64-linux-gnu.tar.gz https://github.com/dogecoin/dogecoin/releases/download/v${DOGECOIN_VER}/dogecoin-${DOGECOIN_VER}-x86_64-linux-gnu.tar.gz

RUN myhash=$(sha256sum "dogecoin-${DOGECOIN_VER}-x86_64-linux-gnu.tar.gz" | cut -d' ' -f1); \
    if [ "$myhash" = "$DOGECOIN_SHASUM" ] ; \
        then echo "checksum ok"; \
        else echo "checksum failed for dogecoin-${DOGECOIN_VER}-x86_64-linux-gnu.tar.gz"; exit 255 ; \
    fi
RUN tar -xvzf dogecoin-${DOGECOIN_VER}-x86_64-linux-gnu.tar.gz
RUN install -C -m 755 -o root -g root --backup=off dogecoin-${DOGECOIN_FOLDER_VER}/bin/* /usr/local/bin/
RUN rm dogecoin-${DOGECOIN_VER}-x86_64-linux-gnu.tar.gz && rm -rf dogecoin-${DOGECOIN_FOLDER_VER}

# Set up dogecoind dirs and files
RUN mkdir -p /root/.dogecoin/
COPY dogecoin.conf /root/.dogecoin/
COPY start.sh /usr/local/bin/start.sh
RUN chmod a+x /usr/local/bin/start.sh

EXPOSE 8335 8336 18335 18336

# NOTE: Defaults to running on mainnet, specify -e TESTNET=1 to start up on testnet
ENTRYPOINT ["start.sh"]
