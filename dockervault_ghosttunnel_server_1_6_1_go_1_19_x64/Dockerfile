FROM dockervault.dn.ipantt.net/dockervault_ubuntu_2004_x64:20220723_001

COPY files/apt-sources.list /etc/apt/sources.list

RUN \
	apt-get -y update && apt-get -y upgrade && \
	apt-get -y install wget curl lsb-release software-properties-common locales apt-transport-https debconf-utils gnupg2 supervisor libc++1 gdb libsss-nss-idmap0 libsasl2-modules-gssapi-mit libc-dbg gdbserver libbabeltrace1 libdw1 libc++1-10 libc++abi1-10 libatomic1 libunwind8 libnuma1 gawk unixodbc && \
	touch /etc/flag_this_is_cloud && \
	touch /etc/flag_this_is_docker && \
	apt-get clean && \
	rm -rf /var/lib/apt/lists/*

RUN \
    wget -O go.tar.gz https://lts.dn.ipantt.net/d/221210_001_18233/go1.19.linux-amd64.tar.gz && \
    tar -C /usr/local -xzf go.tar.gz && \
    rm -f go.tar.gz

RUN \
    mkdir -p /var/opt/ && \
    cd /var/opt/ && \
    wget -O ghostunnel.tar.gz https://lts.dn.ipantt.net/d/221210_002_01321/ghostunnel-v1.6.1.tar.gz && \
    tar xzf ghostunnel.tar.gz && \
    rm -f ghostunnel.tar.gz && \
    export PATH=$PATH:/usr/local/go/bin && \
    make ghostunnel

COPY files/supervisord.conf /etc/supervisor/supervisord.conf

COPY files/ghosttunnel_server.conf /etc/supervisor/ghosttunnel_server.conf

RUN \
	echo "export PS1='[Docker: \u@\h \w]\\$ '" >> /root/.bashrc

EXPOSE 443

CMD ["/usr/bin/supervisord"]


