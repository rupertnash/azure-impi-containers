FROM centos:7.1.1503
LABEL maintainer "Rupert Nash <r.nash@epcc.ed.ac.uk>"

# set up base and ssh keys
COPY ssh_config /root/.ssh/config
COPY sshd_sed_commands /tmp/sshd_sed_commands

RUN \
    yum swap -y fakesystemd systemd \
    && \
    yum install -y epel-release \
    && \
    yum install -y \
	dapl \
	libibverbs \
        libmlx4 \
	librdmacm\
	net-tools \
	numactl \
        openssh-clients \
	openssh-server \
	rdma \
    && \
    yum clean all \
    && \
    mkdir -p /var/run/sshd \
    && \
    ssh-keygen -A \
    && \
    sed -f /tmp/sshd_sed_commands -i /etc/ssh/sshd_config \
    && \
    rm /tmp/sshd_sed_commands \
    && \
    ssh-keygen -f /root/.ssh/id_rsa -t rsa -N '' \
    && \
    chmod 600 /root/.ssh/config \
    && \
    chmod 700 /root/.ssh \
    && \
    cp /root/.ssh/id_rsa.pub /root/.ssh/authorized_keys

# We need the Intel MPI runtime from (registration required):
# https://software.intel.com/en-us/intel-mpi-library/
ADD l_mpi-rt_p_5.1.3.223.tgz /tmp
RUN cd /tmp/l_mpi-rt_p_5.1.3.223 && \
    sed -i -e 's/^ACCEPT_EULA=decline/ACCEPT_EULA=accept/g' silent.cfg && \
    ./install.sh -s silent.cfg && \
    cd .. && \
    rm -rf l_mpi-rt_p_5.1.3.223 intel*.log

# set up sshd on port 23
EXPOSE 23
CMD ["/usr/sbin/sshd", "-D", "-p", "23"]
