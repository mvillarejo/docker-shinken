# Shinken poller with only poller role
#
FROM        shinken-base
MAINTAINER  Manuel Villarejo <mjvillarejo@gmail.com>

RUN	    yum -y install nagios-plugins-all perl-Net-SNMP

#  we just replace 'nagios' group ownership with 'shinken'
RUN         chgrp -R shinken /usr/lib64/nagios/plugins
#  check_icmp require to run as root
RUN         chmod u+s /usr/lib64/nagios/plugins/check_icmp

# dependency for linux-snmp package
RUN         ln -s /usr/lib64/nagios/plugins/utils.pm  /var/lib/shinken/libexec
ADD 	    files/shinken-poller.conf /etc/supervisord.d/

EXPOSE      7771
