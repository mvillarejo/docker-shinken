# Thruk
# 
# VERSION		

FROM        shinken-base
MAINTAINER  Manuel Villarejo <mjvillarejo@gmail.com>

ARG         THRUK_VER
ENV         ENV ${ENV:-mal}
ENV         POD ${POD:-monitoring}
ENV         THRUK_VER ${THRUK_VER:-2.04}
ENV         SUPERVISOR_VER ${SUPERVISOR_VER:-3.2.0}

# prerequisites
RUN         yum -y install python-pip git mod_ldap


# trick to have write access to /sites/shinken host volume 
# RUN         adduser apache -u 1000
 
# install thruk
#           libthruk seems not to be inline with the rest: 2.02-1
RUN         yum -y install http://download.thruk.org/pkg/v$THRUK_VER/rhel7/x86_64/libthruk-2.02-1.rhel7.x86_64.rpm
RUN         yum -y install http://download.thruk.org/pkg/v$THRUK_VER/rhel7/x86_64/thruk-base-$THRUK_VER-1.rhel7.x86_64.rpm
RUN         yum -y install http://download.thruk.org/pkg/v$THRUK_VER/rhel7/x86_64/thruk-plugin-reporting-$THRUK_VER-1.rhel7.x86_64.rpm
RUN         yum -y install http://download.thruk.org/pkg/v$THRUK_VER/rhel7/x86_64/thruk-$THRUK_VER-1.rhel7.x86_64.rpm

# Apache config
RUN         rm /etc/httpd/conf.d/{welcome,userdir,autoindex}.conf
ADD         files/thruk.conf /etc/httpd/conf.d/thruk.conf
RUN         sed -i 's/ENV/'${ENV}'/g' /etc/httpd/conf.d/thruk.conf
RUN         sed -i 's/POD/'${POD}'/g' /etc/httpd/conf.d/thruk.conf


# Thruk config
ADD         files/thruk_local.conf /etc/thruk/thruk_local.conf
RUN         chown apache:apache /etc/thruk/thruk_local.conf




# install supervisor
RUN         pip install supervisor==$SUPERVISOR_VER 
RUN         mkdir -p /etc/supervisord.d 
RUN         mkdir -p /var/log/supervisor
ADD         files/supervisord.conf /etc/supervisord.conf
ADD         files/supervisord_thruk.conf /etc/supervisord.d/supervisord_thruk.conf


EXPOSE      80
CMD         supervisord -c /etc/supervisord.conf -n
