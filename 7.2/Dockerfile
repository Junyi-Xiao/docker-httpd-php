FROM centos:7

MAINTAINER XiaoJunyi <xiaojunyi@foxmail.com>

ENV PHP_VERSION="72"

RUN yum -y --setopt=tsflags=nodocs install epel-release && \
    rpm -ivh http://rpms.famillecollet.com/enterprise/remi-release-7.rpm && \
    yum -y --setopt=tsflags=nodocs update && \
    yum -y --setopt=tsflags=nodocs install httpd httpd-devel gcc make zip unzip sendmail && \
    yum -y --setopt=tsflags=nodocs install openssl openssl-devel mod_ssl && \
    yum -y --setopt=tsflags=nodocs --enablerepo=remi --enablerepo=remi-php${PHP_VERSION} install \
        php php-devel php-mbstring php-mcrypt php-mysqlnd php-pdo php-gd php-opcache php-xml && \
    rpm -ivh https://centos7.iuscommunity.org/ius-release.rpm && \
    yum -y --setopt=tsflags=nodocs install git2u && \
    yum clean all && rm -rf /var/cache/yum && ( \
        mkdir -p /tmp/phpext && cd /tmp/phpext && \
        curl -fsSL 'https://pecl.php.net/get/redis' -o redis.tgz && \
        mkdir -p redis && \
        tar -xf redis.tgz -C redis --strip-components=1 && ( \
            cd redis && \
            phpize && \
            ./configure && \
            make && make install \
        ) \
    ) && ( \
        cd /tmp/phpext && \
        curl -fsSL 'https://pecl.php.net/get/mongodb' -o mongodb.tgz && \
        mkdir -p mongodb && \
        tar -xf mongodb.tgz -C mongodb --strip-components=1 && ( \
            cd mongodb && \
            phpize && \
            ./configure && \
            make && make install \
        ) \
    ) && ( \
        cd /tmp/ && \
        curl -sS https://getcomposer.org/installer | php && \
        chmod +x composer.phar && \
        mv composer.phar /usr/local/bin/composer && \
        composer config -g repo.packagist composer https://packagist.phpcomposer.com \
    ) && \
    rm -rf /tmp/*

COPY php.d/ /etc/php.d/

EXPOSE 80 443

COPY run-httpd.sh /run-httpd.sh
RUN chmod -v +x /run-httpd.sh

CMD ["/run-httpd.sh"]