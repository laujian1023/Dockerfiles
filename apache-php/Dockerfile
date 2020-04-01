FROM centos:latest

RUN yum -y install wget gcc make zlib-devel pcre-devel pcre
RUN wget -P /src https://mirrors.tuna.tsinghua.edu.cn/apache/httpd/httpd-2.4.43.tar.gz
RUN wget -P /src https://mirrors.tuna.tsinghua.edu.cn/apache/apr/apr-1.7.0.tar.gz
RUN wget -P /src https://mirrors.tuna.tsinghua.edu.cn/apache/apr/apr-util-1.6.1.tar.gz
RUN wget -P /src https://www.php.net/distributions/php-7.3.16.tar.gz
RUN cd /src && tar xf httpd-2.4.43.tar.gz && tar xf apr-1.7.0.tar.gz && tar xf apr-util-1.6.1.tar.gz && tar xf php-7.3.16.tar.gz


RUN cd /src/apr-1.7.0 \
&& ./configure --prefix=/usr/local/apr/ && make && make install \
&& cd /src/apr-util-1.6.1 \
&& yum -y install expat-devel \
&& ./configure --prefix=/usr/local/apr-util/ --with-apr=/usr/local/apr/ \
&& make && make install \
&& cd /src/httpd-2.4.43 \
&& ./configure  \
 --prefix=/usr/local/apache/ \
 --with-apr=/usr/local/apr/ \
 --with-apr-util=/usr/local/apr-util/ \
 --enable-so \
 --enable-deflate=shared \
 --enable-expires=shared \
 --enable-rewrite=shared \
 --enable-static-support \
&& make && make install \
&& cd /src/php-7.3.16 \
&& yum -y install libxml2-devel \
&& yum -y install openssl openssl-devel libcurl-devel perl \
&& sed -i '1i\#!/usr/bin/perl -w' /usr/local/apache/bin/apxs \
&& ./configure \
--prefix=/usr/local/php \
--with-pdo-mysql \
--with-curl \
--enable-bcmath \
--with-openssl \
--enable-mbstring \
--enable-opcache \
--with-apxs2=/usr/local/apache/bin/apxs \
&& make && make install \
&& rm -rf /src \
&& ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime 

RUN wget -P  /usr/local/apache/conf  https://raw.githubusercontent.com/tongsq/Dockerfiles/master/apache-php/httpd.conf 
RUN wget -P  /usr/local/php/lib/ https://raw.githubusercontent.com/tongsq/Dockerfiles/master/apache-php/php.ini
EXPOSE 80
CMD ["/usr/local/apache/bin/httpd", "-DFOREGROUND"]
