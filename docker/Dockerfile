FROM node:16.13.1-buster AS build

WORKDIR /tmp
RUN apt update && apt install -y --no-install-recommends \
    build-essential \
    git curl cmake autoconf ca-certificates \
    libpcsclite-dev \
    &&  \
    apt clean && \
    rm -rf /var/lib/apt/lists/*

RUN git clone https://github.com/stz2012/recpt1.git
RUN git clone https://github.com/stz2012/libarib25.git

## COMPILE
RUN cd /tmp/libarib25; \
  git checkout ab6afa7c7f4022af7dda7976489ec7a0716efb9a && \
  cmake . && make && make install

RUN cd /tmp/recpt1/recpt1 && \
  git checkout fd64ce07b285a182b64a92f33a9549c0fa951be5 && \
  ./autogen.sh && \
  ./configure --enable-b25 && \
  make


FROM chinachu/mirakurun:3.9.0-beta.24
COPY --from=build /tmp/libarib25/libarib25.so* /usr/lib/x86_64-linux-gnu/
COPY --from=build /tmp/recpt1/recpt1/recpt1* /usr/local/bin/

