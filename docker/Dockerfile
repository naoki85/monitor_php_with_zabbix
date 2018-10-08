FROM zabbix/zabbix-agent:alpine-3.4.14

RUN apk update && apk add openrc vim curl && \
  sed -i 's/#rc_sys=""/rc_sys="lxc"/g' /etc/rc.conf && \
  echo 'rc_provide="loopback net"' >> /etc/rc.conf && \
  sed -i 's/^#\(rc_logger="YES"\)$/\1/' /etc/rc.conf && \
  sed -i '/tty/d' /etc/inittab && \
  sed -i 's/hostname $opts/# hostname $opts/g' /etc/init.d/hostname && \
  sed -i 's/mount -t tmpfs/# mount -t tmpfs/g' /lib/rc/sh/init.sh && \
  sed -i 's/cgroup_add_service /# cgroup_add_service /g' /lib/rc/sh/openrc-run.sh && \
  mkdir /run/openrc && touch /run/openrc/softlevel
RUN apk add nginx
RUN apk add --update \
  --repository http://dl-cdn.alpinelinux.org/alpine/latest-stable/main \
  --repository http://dl-cdn.alpinelinux.org/alpine/latest-stable/community \
  php7-fpm libbsd=0.8.6-r2
RUN mkdir -p /usr/local/zabbix-agent-ops/bin
COPY ./docker/nginx/nginx.conf /etc/nginx/nginx.conf
COPY ./docker/nginx/default.conf /etc/nginx/conf.d/default.conf
COPY ./docker/php/fpm_default.conf /etc/php7/conf.d/default.conf
COPY ./docker/php/www.conf /etc/php7/php-fpm.d/www.conf
COPY ./docker/zabbix/zabbix_agentd.conf /etc/zabbix/zabbix_agentd.conf
COPY ./docker/zabbix/php-fpm-params.conf /etc/zabbix/zabbix_agentd.d/php-fpm-params.conf
COPY ./docker/zabbix/php-fpm-check.sh /usr/local/zabbix-agent-ops/bin/php-fpm-check.sh
RUN rc-status && rc-service nginx start && rc-service /etc/init.d/php-fpm7 start
EXPOSE 80
EXPOSE 10050

