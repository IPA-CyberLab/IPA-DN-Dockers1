FROM dockervault.dn.ipantt.net/dockervault_ubuntu_2004_arm64:20220723_001

COPY files/apt-sources.list /etc/apt/sources.list

RUN \
    export DEBIAN_FRONTEND=noninteractive && \
	apt-get -y update && apt-get -y upgrade && \
	apt-get -y install wget curl lsb-release software-properties-common locales apt-transport-https debconf-utils gnupg2 supervisor build-essential git && \
	apt-get -y install libssl-dev libpcre3 libpcre3-dev zlib1g zlib1g-dev libgd-dev libxml2 libxml2-dev uuid-dev libxslt1-dev libgeoip-dev && \
	touch /etc/flag_this_is_cloud && \
	touch /etc/flag_this_is_docker && \
	apt-get clean && \
	rm -rf /var/lib/apt/lists/*

RUN \
    mkdir -p /var/opt/ && \
    cd /var/opt/ && \
    git clone --recursive -b IPA-DN-Nginx-1.23.4-patch230404 https://github.com/IPA-CyberLab/IPA-DN-Fork-Nginx-Patch.git && \
    git clone --recursive https://github.com/DN-Forks/ngx-fancyindex.git && \
    cd /var/opt/ngx-fancyindex/ && \
    git checkout 5930c29d6a97787c101cee63b62f6eaa2945bd96

# http://nginx.org/en/docs/configure.html

RUN \
	mkdir -p /var/opt/openssl_src/ && \
	cd /var/opt/openssl_src/ && \
	curl --insecure --pinnedpubkey "sha256//lvnOVgA0u06WySztudkn+urQda/zFBRd65A5wCmcBpQ=" https://static2.lts.dn.ipantt.net/d/230404_001_openssl-3_0_8_19584/openssl-3.0.8.tar.gz -o /var/opt/openssl_src/openssl_src.tar.gz && \
	tar xzf openssl_src.tar.gz && \
    cd /var/opt/IPA-DN-Fork-Nginx-Patch/ && \
    auto/configure   --with-openssl=/var/opt/openssl_src/openssl-3.0.8/ --with-openssl-opt='-static threads no-hw no-engine no-shared no-dso enable-weak-ssl-ciphers enable-ssl3 enable-ssl3-method no-async no-tests linux-aarch64'   --with-cc-opt='-g -O2 -fstack-protector-strong -Wformat -Werror=format-security -fPIC -Wdate-time -D_FORTIFY_SOURCE=2' --with-ld-opt='-Wl,-Bsymbolic-functions -Wl,-z,relro -Wl,-z,now -fPIC'    --prefix=/usr/share/nginx --sbin-path=/usr/sbin/nginx --conf-path=/etc/nginx/nginx.conf --http-log-path=/var/log/nginx/access.log --error-log-path=/var/log/nginx/error.log --lock-path=/var/lock/nginx.lock --pid-path=/run/nginx.pid    --modules-path=/usr/lib/nginx/modules --http-client-body-temp-path=/var/lib/nginx/body --http-fastcgi-temp-path=/var/lib/nginx/fastcgi --http-proxy-temp-path=/var/lib/nginx/proxy --http-scgi-temp-path=/var/lib/nginx/scgi    --http-uwsgi-temp-path=/var/lib/nginx/uwsgi --with-debug --with-compat --with-pcre-jit --with-http_ssl_module --with-http_stub_status_module --with-http_realip_module --with-http_auth_request_module --with-http_v2_module    --with-http_dav_module --with-http_slice_module --with-threads --with-http_addition_module --with-http_gunzip_module --with-http_gzip_static_module --with-http_image_filter_module=dynamic --with-http_sub_module    --with-http_xslt_module=dynamic --with-stream=dynamic --with-stream_ssl_module --with-mail=dynamic --with-mail_ssl_module --with-http_geoip_module=dynamic --with-http_random_index_module --with-http_dav_module --with-http_flv_module --with-http_mp4_module --with-http_secure_link_module --with-stream_realip_module --with-stream_geoip_module=dynamic --with-stream_ssl_preread_module   --add-module=/var/opt/ngx-fancyindex/ && make -j 4 && make install && ln -s /dev/stderr /var/log/nginx/error.log && ln -s /dev/stdout /var/log/nginx/access.log && rm -fr /var/opt/openssl_src/ && mkdir -p /var/lib/nginx/ && cd / && rm -fr /var/opt/IPA-DN-Nginx-1.23.4-patch230404/ && rm -fr /var/opt/ngx-fancyindex/

COPY files/supervisord.conf /etc/supervisor/supervisord.conf

COPY files/nginx.conf /etc/supervisor/conf.d/nginx.conf

COPY files/openssl.cnf /etc/ssl/openssl.cnf

RUN \
	echo "export PS1='[Docker: \u@\h \w]\\$ '" >> /root/.bashrc

EXPOSE 80 443

CMD ["/usr/bin/supervisord"]

