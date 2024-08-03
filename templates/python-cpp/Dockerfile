FROM ubuntu:20.04

ENV DEBIAN_FRONTEND="noninteractive"

RUN apt update && apt install -y --no-install-recommends \
    build-essential \
    git \
    git-lfs \
    curl \
    wget \
    make \
    libtool \
    autoconf \
    unzip \
    openssh-server \
    gdb \
    && apt clean all

# cmake
RUN mkdir -p ~/dists && \
    cd ~/dists && \
    wget --no-check-certificate https://github.com/Kitware/CMake/releases/download/v3.30.2/cmake-3.30.2-linux-x86_64.tar.gz && \
    tar -zxvf cmake-3.30.2-linux-x86_64.tar.gz && \
    rm cmake-3.30.2-linux-x86_64.tar.gz && \
    ln -s $(pwd)/cmake-3.30.2-linux-x86_64/bin/cmake /usr/local/bin/cmake

# sshd
RUN mkdir /var/run/sshd && \
    echo 'root:root' | chpasswd && \
    sed -i 's/#PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config && \
    sed -i 's/#PasswordAuthentication yes/PasswordAuthentication yes/' /etc/ssh/sshd_config
EXPOSE 22

COPY work /root/work
WORKDIR /root/work

CMD ["/usr/sbin/sshd", "-D"]
