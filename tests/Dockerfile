FROM fedora:latest

RUN dnf -y install 'dnf-command(builddep)' && \
    dnf -y builddep gnutls && \
    dnf -y install ninja-build cmake automake autoconf bison gperf libtool patch

RUN mkdir /install
RUN git clone https://github.com/open-quantum-safe/liboqs.git && \
    cd liboqs && \
    cmake -DCMAKE_INSTALL_PREFIX=/install -DOQS_USE_OPENSSL=OFF -DBUILD_SHARED_LIBS=ON -S . -B _build && \
    cmake --build _build && \
    sudo cmake --install _build

RUN git clone -b wip/dueno/dilithium-test https://gitlab.com/dueno/gnutls.git && \
    cd gnutls && \
    git submodule update --init && \
    ./bootstrap --skip-po && \
    ./configure --disable-doc --enable-pqc --prefix=/install --disable-full-test-suite \
	          CFLAGS="-I/install/include" \
	          LDFLAGS="-L/install/lib64 -Wl,-rpath,/install/lib64" && \
    make -j$(nproc) && \
    sudo make install
