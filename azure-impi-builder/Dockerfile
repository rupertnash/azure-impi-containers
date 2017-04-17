FROM impi-centos-runtime
LABEL maintainer "Rupert Nash <r.nash@epcc.ed.ac.uk>"

RUN yum install -y centos-release-scl &&\
    yum install -y devtoolset-6 &&\
    yum clean all

COPY env.sh /etc/profile.d/env.sh

