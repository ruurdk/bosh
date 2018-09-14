FROM arm64v8/ubuntu:14.04

RUN locale-gen en_US.UTF-8
RUN dpkg-reconfigure locales
ENV LANG en_US.UTF-8
ENV LANGUAGE en_US.UTF-8
ENV LC_ALL en_US.UTF-8

RUN apt-get update
RUN apt-get -y upgrade && apt-get clean
RUN apt-get install -y \
	build-essential \
	git \
	curl \
	wget \
	tar \
	libssl-dev \
	libreadline-dev \
	dnsutils \
	xvfb \
	realpath \
	&& apt-get clean
RUN cd /tmp \
        && wget https://github.com/stedolan/jq/releases/download/jq-1.5/jq-1.5.tar.gz \
        && tar xfvz jq-1.5.tar.gz \
        && cd jq-1.5 \
        && ./configure && make && make install \
        && rm -rf /tmp/jq-1.5


# Nokogiri dependencies
RUN apt-get install -y libxslt-dev libxml2-dev && apt-get clean

ADD install-ruby.sh /tmp/install-ruby.sh
RUN chmod a+x /tmp/install-ruby.sh
RUN cd /tmp && ./install-ruby.sh && rm install-ruby.sh

COPY --from=arm64v8/golang:1 /usr/local/go /usr/local/go
ENV GOROOT=/usr/local/go PATH=/usr/local/go/bin:$PATH
