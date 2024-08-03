FROM ubuntu:20.04

ENV DEBIAN_FRONTEND="noninteractive"

RUN apt update && \
    apt install -y --no-install-recommends \
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
    lsb-release \
    software-properties-common \
    gnupg \
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

# clang
RUN wget --no-check-certificate https://apt.llvm.org/llvm.sh && \
    chmod +x llvm.sh && \
    ./llvm.sh 18 && \
    rm llvm.sh && \
    update-alternatives --install /usr/bin/clang clang /usr/bin/clang-18 100 && \
    update-alternatives --install /usr/bin/clang++ clang++ /usr/bin/clang++-18 100

# C++ after install clang
RUN apt -y update && \
    apt install -y --no-install-recommends \
    libc++-18-dev \
    libc++abi-18-dev \
    && apt clean all

COPY work /root/work
WORKDIR /root/work

CMD ["/usr/sbin/sshd", "-D"]
