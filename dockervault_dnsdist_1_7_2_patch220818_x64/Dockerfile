FROM dockervault.dn.ipantt.net/dockervault_ubuntu_2004_x64:20220723_001

COPY files/apt-sources.list /etc/apt/sources.list

RUN \
    export DEBIAN_FRONTEND=noninteractive && \
	apt-get -y update && apt-get -y upgrade && \
	apt-get -y install wget curl lsb-release software-properties-common locales apt-transport-https debconf-utils gnupg2 supervisor build-essential git libedit-dev libboost-all-dev luajit lua5.3 luarocks ragel python3.8-venv && \
	touch /etc/flag_this_is_cloud && \
	touch /etc/flag_this_is_docker && \
	apt-get clean && \
	rm -rf /var/lib/apt/lists/*

RUN \
    mkdir -p /var/opt/ && \
    cd /var/opt/ && \
    git clone --recursive -b IPA-DN-dnsdist-1.7.2-patch220818 https://github.com/IPA-CyberLab/IPA-DN-Fork-PDNS-DnsDist-Patch.git && \
    cd /var/opt/IPA-DN-Fork-PDNS-DnsDist-Patch/pdns/dnsdistdist/ && \
    autoreconf -vi && ./configure && make -j 4 && make install && \
    cd / && \
    rm -fr /var/opt/IPA-DN-Fork-PDNS-DnsDist-Patch/

COPY files/supervisord.conf /etc/supervisor/supervisord.conf

COPY files/dnsdist.conf /etc/supervisor/conf.d/dnsdist.conf

COPY files/openssl.cnf /etc/ssl/openssl.cnf

RUN \
	echo "export PS1='[Docker: \u@\h \w]\\$ '" >> /root/.bashrc

EXPOSE 80 53/udp

CMD ["/usr/bin/supervisord"]


