FROM ptipd/uin-devs-ubuntu:18.04-apache2-php7

RUN printf "Build of ptipd/uin-ssos-ubuntu:18.04-apache2-php7-llng2, date: %s\n" `date -u +"%Y-%m-%dT%H:%M:%SZ"` >> /etc/BUILD && \
    #
    # install required perl modules:
    wget -O - https://lemonldap-ng.org/_media/rpm-gpg-key-ow2 | apt-key add - && \
    echo "# LemonLDAP::NG repository\n \
deb     https://lemonldap-ng.org/deb stable main\n \
deb-src https://lemonldap-ng.org/deb stable main\n" \
        > /etc/apt/sources.list.d/lemonldap-ng.list && \
    cat /etc/apt/sources.list.d/lemonldap-ng.list && \
    apt-get -y update && \
    DEBIAN_FRONTEND=noninteractive DEBCONF_NONINTERACTIVE_SEEN=true \
        apt-get -y install --no-install-recommends liblemonldap-ng-handler-perl && \
    #
    # cleanup:
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*
