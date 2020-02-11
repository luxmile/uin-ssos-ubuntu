FROM ptipd/uin-base-ubuntu:1.3_18.04

ARG CFG_ID
ARG APP_BUILD

RUN printf "Build of ${APP_BUILD}, date: %s\n" `date -u +"%Y-%m-%dT%H:%M:%SZ"` >> /etc/BUILD && \
    #
    # install web server and required perl modules:
    apt-get -y update && \
    apt-get -y install --no-install-recommends apache2 software-properties-common && \
    wget -O - https://lemonldap-ng.org/_media/rpm-gpg-key-ow2 | apt-key add - && \
    echo "# LemonLDAP::NG repository\n \
deb     https://lemonldap-ng.org/deb stable main\n \
deb-src https://lemonldap-ng.org/deb stable main\n" \
        > /etc/apt/sources.list.d/lemonldap-ng.list && \
    chown 0:0 /etc/apt/sources.list.d/lemonldap-ng.list && \
    chmod a+r /etc/apt/sources.list.d/lemonldap-ng.list && \
    apt-get -y update && \
    DEBIAN_FRONTEND=noninteractive DEBCONF_NONINTERACTIVE_SEEN=true \
        apt-get -y install libapache2-mod-perl2 libapache2-mod-fcgid lemonldap-ng libnet-ldap-perl libredis-perl && \
    a2enmod fcgid perl alias rewrite headers && \
    #
    # prepare custom apache settings:
    groupadd -g ${CFG_ID} cfg && \
    useradd -u ${CFG_ID} -g cfg -s /usr/sbin/nologin -d /opt cfg && \
    sed -i 's/80/0.0.0.0:80/' /etc/apache2/ports.conf && \
    sed -i 's/443/0.0.0.0:443/' /etc/apache2/ports.conf && \
    echo "#!/bin/sh\n\
set -e\n\
rm -f /run/apache2/apache2.pid\n\
exec apache2ctl -DFOREGROUND" > /app && \
    chmod 750 /app && \
    #
    # cleanup:
    apt-get clean && \
    rm -rf /var/lib/apt/lists/* /usr/share/doc/* /usr/share/man/* && \
    rm -f /var/log/*log /var/log/apache2/*log var/log/apt/*log 

EXPOSE 80
STOPSIGNAL SIGWINCH

ENTRYPOINT /app
