# Shinken scheduler with only scheduler role
#
FROM        shinken-base
MAINTAINER  Manuel Villarejo <mjvillarejo@gmail.com>

ADD         files/shinken-scheduler.conf /etc/supervisord.d/


# Retention memcache
RUN         yum -y install python-memcached
RUN         su - shinken -c "shinken install retention-memcache"

EXPOSE      7768
