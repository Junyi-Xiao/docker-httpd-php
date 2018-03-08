FROM centos:7

MAINTAINER XiaoJunyi <xiaojunyi@foxmail.com>

ENV PHP_VERSION="72"

RUN yum -y --setopt=tsflags=nodocs install epel-release && \
    rpm -ivh http://rpms.famillecollet.com/enterprise/remi-release-7.rpm && \
    yum -y --setopt=tsflags=nodocs update && \
    yum -y --setopt=tsflags=nodocs install httpd httpd-devel gcc make && \
    yum -y --setopt=tsflags=nodocs install openssl openssl-devel mod_ssl && \
    yum -y --setopt=tsflags=nodocs --enablerepo=remi --enablerepo=remi-php${PHP_VERSION} install \
        php php-devel php-mbstring php-mcrypt php-mysqlnd php-pdo php-gd php-opcache php-xml && \
    yum clean all && rm -rf /var/cache/yum && rm -rf /tmp/yum* && ( \
        mkdir -p /tmp/phpext && cd /tmp/phpext && \
        curl -fsSL 'https://pecl.php.net/get/redis' -o redis.tgz && \
        mkdir -p redis && \
        tar -xf redis.tgz -C redis --strip-components=1 && ( \
            cd redis && \
            phpize && \
            ./configure && \
            make && \
            make install \
        ) \
    ) && \
    rm -rf /tmp/phpext
ADD php.d/redis.ini /etc/php.d/redis.ini

EXPOSE 80 443

ADD run-httpd.sh /run-httpd.sh
RUN chmod -v +x /run-httpd.sh

CMD ["/run-httpd.sh"]