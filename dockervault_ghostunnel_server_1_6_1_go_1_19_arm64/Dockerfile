FROM dockervault.dn.ipantt.net/dockervault_ubuntu_2004_arm64:20221210_001

COPY files/apt-sources.list /etc/apt/sources.list

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
    git clone -b 221211_v1.6.1_enable_legacy_ssl --recursive https://github.com/dnobori/DN-fork-ghostunnel-221211.git && \
    mv DN-fork-ghostunnel-221211 ghostunnel && \
    cd /var/opt/ghostunnel/ && \
    export PATH=$PATH:/usr/local/go/bin && \
    make ghostunnel

COPY files/supervisord.conf /etc/supervisor/supervisord.conf

COPY files/ghostunnel_server.conf /etc/supervisor/conf.d/ghostunnel_server.conf

COPY files/openssl.cnf /etc/ssl/openssl.cnf

RUN \
	echo "export PS1='[Docker: \u@\h \w]\\$ '" >> /root/.bashrc

EXPOSE 443

CMD ["/usr/bin/supervisord"]


