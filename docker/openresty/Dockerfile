FROM openresty/openresty:1.11.2.2-jessie

RUN apt-get update
RUN apt-get install -y --no-install-recommends \
    apt-file \
    luarocks \
    libc6-dev \
    libssl-dev \
    g++ \
    libtool \
    automake \
    autoconf \
    liblua5.1-0-dev liblua5.2-dev liblua50-dev libluajit-5.1-dev \
    dnsmasq \
    git \
    systemtap

RUN /usr/local/openresty/bin/opm install openresty/lua-resty-redis
RUN /usr/local/openresty/bin/opm install agentzh/lua-resty-http

RUN /usr/local/openresty/luajit/bin/luarocks install luautf8
RUN /usr/local/openresty/luajit/bin/luarocks install lua-cjson
RUN /usr/local/openresty/luajit/bin/luarocks install lua-resty-mysql
RUN /usr/local/openresty/luajit/bin/luarocks install lua-resty-string
RUN /usr/local/openresty/luajit/bin/luarocks install luasocket
RUN /usr/local/openresty/luajit/bin/luarocks install luacrypto
RUN /usr/local/openresty/luajit/bin/luarocks install luasec
RUN /usr/local/openresty/luajit/bin/luarocks install nats
RUN luarocks install luasocket
RUN /usr/local/openresty/luajit/bin/luarocks install net-url
RUN cd && git clone https://github.com/openresty/openresty-systemtap-toolkit.git
RUN cd && git clone https://github.com/openresty/stapxx.git

RUN cd /tmp && \
    git clone https://github.com/aerospike/aerospike-client-c.git && \
    cd aerospike-client-c && git submodule update --init && \
    make EVENT_LIB=libuv && make clean && make EVENT_LIB=libuv test && make install

#RUN cd /tmp && \
#    git clone https://github.com/aerospike/client-lua.git && \
#    cd client-lua && \
#    ./build_linux.sh

RUN sed -i '/http {/ a\ include conf.d/*;' /usr/local/openresty/nginx/conf/nginx.conf && \
    mkdir -p /usr/local/openresty/nginx/conf/conf.d && \
    echo '#!/bin/bash' >> /opt/entry-point.sh && \
#    echo 'service dnsmasq start' >> /opt/entry-point.sh && \
    echo '/usr/local/openresty/bin/openresty -g "daemon off;"' >> /opt/entry-point.sh

ENTRYPOINT ["/bin/bash", "/opt/entry-point.sh"]
