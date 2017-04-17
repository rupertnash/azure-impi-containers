FROM centos:7
LABEL maintainer "Rupert Nash <r.nash@epcc.ed.ac.uk>"

# set up base and ssh keys
COPY ssh_config /root/.ssh/config

RUN yum swap -y fakesystemd systemd \
    && yum install -y epel-release \
    && yum install -y \
        openssh-clients openssh-server net-tools numactl \
        libmlx4 librdmacm libibverbs dapl rdma \
    && yum clean all \
    && mkdir -p /var/run/sshd \
    && ssh-keygen -A \
    && sed -i 's/UsePAM yes/UsePAM no/g' /etc/ssh/sshd_config \
    && sed -i 's/#PermitRootLogin yes/PermitRootLogin yes/g' /etc/ssh/sshd_config \
    && sed -i 's/#RSAAuthentication yes/RSAAuthentication yes/g' /etc/ssh/sshd_config \
    && sed -i 's/#PubkeyAuthentication yes/PubkeyAuthentication yes/g' /etc/ssh/sshd_config \
    && ssh-keygen -f /root/.ssh/id_rsa -t rsa -N '' \
    && chmod 600 /root/.ssh/config \
    && chmod 700 /root/.ssh \
    && cp /root/.ssh/id_rsa.pub /root/.ssh/authorized_keys

# We need the Intel MPI library from (registration required):
# https://software.intel.com/en-us/intel-mpi-library/
WORKDIR /tmp
ADD l_mpi_2017.2.174.tar.gz /tmp
RUN cd l_mpi_2017.2.174 && \
    sed -i -e 's/^ACCEPT_EULA=decline/ACCEPT_EULA=accept/g' silent.cfg && \
    ./install.sh -s silent.cfg && \
    cd .. && \
    rm -rf l_mpi_2017.2.174 intel*.log

# set up sshd on port 23
EXPOSE 23
CMD ["/usr/sbin/sshd", "-D", "-p", "23"]
