# Shinken arbiter with only arbiter role
#
FROM        shinken-base
MAINTAINER  Manuel Villarejo <mjvillarejo@gmail.com>

RUN         yum install inotify-tools -y
# Add shinken config watcher to restart arbiter, when changes happen
ADD         files/watch_shinken_config.sh /usr/bin/watch_shinken_config.sh
ADD         files/shinken-config-watcher.conf /etc/supervisord.d/

ADD         files/shinken-arbiter.conf /etc/supervisord.d/

EXPOSE      7770
