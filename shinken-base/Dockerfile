# Shinken Base for building others shinken roles
# 
FROM        centos:centos7
MAINTAINER  Manuel Villarejo <mjvillarejo@gmail.com>

ENV         SHINKEN_VER ${SHINKEN_VER:-2.4.2}
ENV         SUPERVISOR_VER ${SUPERVISOR_VER:-3.2.0}
ENV         ENV ${ENV:-mal}
ENV         POD ${POD:-monitoring}

# datetime set 
RUN         unlink /etc/localtime && ln -s /usr/share/zoneinfo/America/New_York /etc/localtime

# prerequisites
RUN	    yum clean all && yum -y update
RUN         rpm -ivh http://mirrors.ircam.fr/pub/fedora/epel/7/x86_64/e/epel-release-7-5.noarch.rpm
RUN         yum -y install python-crypto python-pip python-setuptools vim-enhanced wget
# modules depenencies
RUN         yum -y install python-pymongo

# install shinken
RUN         useradd shinken



ADD         https://github.com/naparuba/shinken/archive/$SHINKEN_VER.tar.gz  /shinken-$SHINKEN_VER.tar.gz

RUN         tar xzvf /shinken-$SHINKEN_VER.tar.gz && cd shinken-$SHINKEN_VER && python setup.py install
RUN         rm -Rf /shinken-$SHINKEN_VER.tar.gz

# create folders
RUN         mkdir -p /sites/shinken /sites/run/shinken /sites/log/shinken /sites/shinken/config/packs /sites/lib/shinken/{cli,doc,inventory,libexec,modules,share,packs}  
RUN         chown -R shinken:shinken /sites

# install modules and packs
RUN         su - shinken -c "shinken --init"
# etc = /sites/shinken but lib = /var/lib/shinken
RUN         su - shinken -c "sed -i 's|\/etc\/shinken|\/sites\/shinken|' /home/shinken/.shinken.ini"
RUN         su - shinken -c "sed -i 's|\/packs|\/config\/packs|' /home/shinken/.shinken.ini"
RUN         su - shinken -c "shinken install webui"
RUN         su - shinken -c "shinken install mod-mongodb"

RUN         su - shinken -c "shinken install ui-graphite"
RUN         su - shinken -c "shinken install graphite"

RUN         su - shinken -c "shinken install livestatus"
RUN         su - shinken -c "shinken install auth-cfg-password"
RUN         su - shinken -c "shinken install auth-active-directory"
# varnish
# memcached
# mysql
# postgresql
RUN         su - shinken -c "shinken install logstore-sqlite"
RUN         su - shinken -c "shinken install logstore-mongodb"
RUN         su - shinken -c "shinken install linux-snmp"

RUN         chown -R shinken:shinken /sites


# install supervisor
RUN         pip install supervisor==$SUPERVISOR_VER
RUN         mkdir -p /etc/supervisord.d
RUN         mkdir -p /var/log/supervisor
ADD         files/supervisord.conf /etc/supervisord.conf

# launch command
CMD         supervisord -c /etc/supervisord.conf -n

