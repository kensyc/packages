FROM debian:11-slim AS os

RUN apt-get update && apt-get install -y \
    sudo \
    git \
    ninja-build \
    gettext \
    libtool \
    libtool-bin \
    autoconf \
    automake \
    cmake \
    g++ \
    pkg-config \
    unzip \
    curl \
    doxygen \
    python \
    python3-pip \
    man \
    universal-ctags

# RUN curl -OL https://go.dev/dl/go1.18.3.linux-amd64.tar.gz; \
#     rm -rf /usr/local/go; \
#     tar -C /usr/local -xzf go1.18.3.linux-amd64.tar.gz

RUN git clone https://github.com/neovim/neovim && \
    cd neovim && \
    make && \
    sudo make install

RUN curl -fsSL https://deb.nodesource.com/setup_18.x | bash - && \
    apt-get install -y nodejs

RUN useradd -m -u 1000 nvim

USER 1000
ENV XDG_CACHE_HOME=/home/nvim/.cache
# ENV PATH=/home/nvim/.local/bin:/usr/local/go/bin:$PATH

FROM os as workspace
RUN mkdir /home/nvim/.config
RUN git clone https://github.com/kensyc/dotfiles.nvim /home/nvim/.config/nvim && \
        /home/nvim/.config/nvim/install 2>&1

WORKDIR /workdir

ENTRYPOINT ["nvim"]
