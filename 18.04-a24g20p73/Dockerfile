FROM ptipd/uin-devs-ubuntu:1.5_18.04-apache2.4-php7.3

ARG APP_BUILD

RUN printf "Build of ${APP_BUILD}, date: %s\n" `date -u +"%Y-%m-%dT%H:%M:%SZ"` >> /etc/BUILD && \
    #
    # install required perl modules:
    wget -O - https://lemonldap-ng.org/_media/rpm-gpg-key-ow2 | apt-key add - && \
    echo "# LemonLDAP::NG repository\n \
deb     https://lemonldap-ng.org/deb stable main\n \
deb-src https://lemonldap-ng.org/deb stable main\n" \
        > /etc/apt/sources.list.d/lemonldap-ng.list && \
    chown 0:0 /etc/apt/sources.list.d/lemonldap-ng.list && \
    chmod a+r /etc/apt/sources.list.d/lemonldap-ng.list && \
    apt-get -y update && \
    DEBIAN_FRONTEND=noninteractive DEBCONF_NONINTERACTIVE_SEEN=true \
        apt-get -y install libapache2-mod-perl2 libapache2-mod-fcgid liblemonldap-ng-handler-perl libnet-ldap-perl libredis-perl && \
    a2enmod fcgid perl alias rewrite headers && \
    #
    # cleanup:
    apt-get clean && \
    rm -rf /var/lib/apt/lists/* /usr/share/doc/* /usr/share/man/* && \
    rm -f /var/log/*log /var/log/apache2/*log var/log/apt/*log 
