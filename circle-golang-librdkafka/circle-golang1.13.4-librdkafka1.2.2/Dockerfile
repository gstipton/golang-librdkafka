FROM circleci/golang:1.13.4

ARG LIBRDKAFKA_VERSION=1.2.2

# E: Failed to fetch http://deb.debian.org/debian/pool/main/p/python-cryptography/python3-cryptography_2.6.1-3_amd64.deb  404  Not Found [IP: 151.101.36.204 80]
# E: Unable to fetch some archives, maybe run apt-get update or try with --fix-missing?
RUN sudo apt-get update

RUN sudo apt-get install python3-pip gettext-base
RUN sudo pip3 install --no-cache-dir 'awscli>=1.15.50'
RUN go get -u github.com/rubenv/sql-migrate/...

RUN cd /tmp

RUN wget -O "librdkafka.tar.gz" "https://github.com/edenhill/librdkafka/archive/v${LIBRDKAFKA_VERSION}.tar.gz"

RUN mkdir -p librdkafka

RUN tar \
  --extract \
  --file "librdkafka.tar.gz" \
  --directory "librdkafka" \
  --strip-components 1

RUN cd "librdkafka" && \
  ./configure --prefix=/usr && \
  make && \
  sudo make install

ENV CGO_ENABLED=1 \
  GOOS=linux \
  GOARCH=amd64