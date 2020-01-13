FROM ptipd/uin-base-ubuntu:18.04

RUN printf "Build of ptipd/uin-ssos-ubuntu:18.04-apache2-llng2, date: %s\n" `date -u +"%Y-%m-%dT%H:%M:%SZ"` >> /etc/BUILD && \
    #
    # install required perl modules:
    apt-get -y update && \
    apt-get -y install --no-install-recommends apache2 software-properties-common gpg-agent && \
    wget -O - https://lemonldap-ng.org/_media/rpm-gpg-key-ow2 | apt-key add - && \
    echo "# LemonLDAP::NG repository\n \
deb     https://lemonldap-ng.org/deb stable main\n \
deb-src https://lemonldap-ng.org/deb stable main\n" \
        > /etc/apt/sources.list.d/lemonldap-ng.list && \
    chown 0:0 /etc/apt/sources.list.d/lemonldap-ng.list && \
    chmod a+r /etc/apt/sources.list.d/lemonldap-ng.list && \
    apt-get -y update && \
    DEBIAN_FRONTEND=noninteractive DEBCONF_NONINTERACTIVE_SEEN=true \
        apt-get -y install --no-install-recommends \
        libapache2-mod-perl2 libapache2-mod-fcgid lemonldap-ng lemonldap-ng-handler && \
    a2enmod fcgid perl alias rewrite headers && \
    #
    # cleanup:
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

COPY base/entpoint.sh /app

EXPOSE 80
STOPSIGNAL SIGWINCH

ENTRYPOINT /app
