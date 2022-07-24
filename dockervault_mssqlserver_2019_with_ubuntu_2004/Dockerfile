FROM dockervault.dn.ipantt.net/dockervault_ubuntu_2004_x64:20220723_001

COPY files/apt-sources.list /etc/apt/sources.list

RUN \
	apt-get -y update && \
	apt-get -y install wget curl lsb-release software-properties-common locales apt-transport-https debconf-utils gnupg2 supervisor libc++1 gdb libsss-nss-idmap0 libsasl2-modules-gssapi-mit libc-dbg gdbserver libbabeltrace1 libdw1 libc++1-10 libc++abi1-10 libatomic1 libunwind8 libnuma1 gawk unixodbc && \
	touch /etc/flag_this_is_cloud && \
	touch /etc/flag_this_is_docker && \
	apt-get clean && \
	rm -rf /var/lib/apt/lists/*

RUN \
	wget -O mssql.deb https://user220724001:passcKwOcYRrySo208ZTlHxoHz7f@lts.dn.ipantt.net/d/220724_001_ms-sql-server-2019-express_qxynq50h/auth9192412/mssql-server_15.0.4236.7-1_amd64.deb && \
	dpkg -i mssql.deb && \
	rm -f mssql.deb && \
	rm -fr /var/opt/mssql/

RUN \
	wget -O msodbc.deb https://user220724001:passcKwOcYRrySo208ZTlHxoHz7f@lts.dn.ipantt.net/d/220724_001_ms-sql-server-2019-express_qxynq50h/auth9192412/msodbcsql17_17.10.1.1-1_amd64.deb && \
	yes | dpkg -i msodbc.deb && \
	rm -f msodbc.deb

RUN \
    wget -O mssql-tools.deb https://user220724001:passcKwOcYRrySo208ZTlHxoHz7f@lts.dn.ipantt.net/d/220724_001_ms-sql-server-2019-express_qxynq50h/auth9192412/mssql-tools_17.10.1.1-1_amd64.deb && \
    yes | dpkg -i mssql-tools.deb && \
    rm -f mssql-tools.deb

RUN \
    wget -O SQL_Server_2019_Express_License_Agreement_Docs.zip https://user220724001:passcKwOcYRrySo208ZTlHxoHz7f@lts.dn.ipantt.net/d/220724_001_ms-sql-server-2019-express_qxynq50h/auth9192412/SQL_Server_2019_Express_License_Agreement_Docs.zip

COPY files/supervisord.conf /etc/supervisor/supervisord.conf

COPY files/sqlserver.conf /etc/supervisor/conf.d/sqlserver.conf

EXPOSE 1433

CMD ["/usr/bin/supervisord"]


