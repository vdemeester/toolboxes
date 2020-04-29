FROM registry.fedoraproject.org/fedora:30

ENV NAME=fedora-toolbox VERSION=30
LABEL com.github.containers.toolbox="true" \
      com.github.debarshiray.toolbox="true" \
      com.redhat.component="$NAME" \
      name="$FGC/$NAME" \
      version="$VERSION" \
      usage="This image is meant to be used with the toolbox command" \
      summary="Base image for creating Fedora toolbox containers" \
      maintainer="Debarshi Ray <rishi@fedoraproject.org>"

RUN sed -i '/tsflags=nodocs/d' /etc/dnf/dnf.conf
RUN dnf -y swap coreutils-single coreutils-full

COPY missing-docs /
RUN dnf -y reinstall $(<missing-docs)
RUN rm /missing-docs

COPY extra-packages /
RUN dnf -y install $(<extra-packages)
RUN rm /extra-packages

RUN dnf -y install krb5-workstation
COPY krb5.conf /etc/krb5.conf

ENV CERTS_RPM=redhat-internal-cert-install-0.1-18.el7.csb.noarch.rpm
RUN wget http://hdn.corp.redhat.com/rhel7-csb-stage/RPMS/noarch/${CERTS_RPM} && \
    rpm -i ${CERTS_RPM} && \
    wget https://download.devel.redhat.com/rel-eng/RCMTOOLS/rcm-tools-fedora.repo -O /etc/yum.repos.d/rcm-tools-fedora.repo && \
    dnf install -y rhpkg

RUN dnf clean all

CMD /bin/sh
