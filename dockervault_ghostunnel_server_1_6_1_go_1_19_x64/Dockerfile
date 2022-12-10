FROM dockervault.dn.ipantt.net/dockervault_ubuntu_2004_x64:20220723_001

COPY files/apt-sources.list /etc/apt/sources.list
COPY files/openssl.cnf /etc/ssl/openssl.cnf

RUN \
    export DEBIAN_FRONTEND=noninteractive && \
	apt-get -y update && apt-get -y upgrade && \
	apt-get -y install wget curl lsb-release software-properties-common locales apt-transport-https debconf-utils gnupg2 supervisor build-essential git && \
	touch /etc/flag_this_is_cloud && \
	touch /etc/flag_this_is_docker && \
	apt-get clean && \
	rm -rf /var/lib/apt/lists/*

RUN \
    wget -O go.tar.gz https://lts.dn.ipantt.net/d/221002_001_go1_19_28904/go1.19.linux-$(dpkg --print-architecture).tar.gz && \
    tar -C /usr/local -xzf go.tar.gz && \
    rm -f go.tar.gz

RUN \
    mkdir -p /var/opt/ && \
    cd /var/opt/ && \
    wget -O ghostunnel.tar.gz https://lts.dn.ipantt.net/d/221210_002_01321/ghostunnel-v1.6.1.tar.gz && \
    tar xzf ghostunnel.tar.gz && \
    rm -f ghostunnel.tar.gz && \
    cd /var/opt/ghostunnel/ && \
    export PATH=$PATH:/usr/local/go/bin && \
    make ghostunnel

COPY files/supervisord.conf /etc/supervisor/supervisord.conf

COPY files/ghostunnel_server.conf /etc/supervisor/conf.d/ghostunnel_server.conf

RUN \
	echo "export PS1='[Docker: \u@\h \w]\\$ '" >> /root/.bashrc

EXPOSE 443

CMD ["/usr/bin/supervisord"]


