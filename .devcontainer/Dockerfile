FROM mcr.microsoft.com/devcontainers/base:1.1-ubuntu24.04

# Install prerequisites and fish shell
RUN apt-get -y update \
    && apt-get -y install fish zoxide \
    && apt install -y build-essential curl libffi-dev libffi8ubuntu1 libgmp-dev libgmp10 libncurses-dev \
    && apt-get -y clean \
    && rm -rf /var/lib/apt/lists/* /var/cache/apt/archives/* \
    && fish -c 'echo' \
    && echo " zoxide init fish | source" > ${HOME}/.config/fish/config.fish 
    
# Installing Oh-My-Fish
ADD --chown=vscode:vscode --chmod=644 https://raw.githubusercontent.com/oh-my-fish/oh-my-fish/master/bin/install /install
RUN fish /install --noninteractive \
    && fish -c 'omf install plain' \
    && rm -f /install

# Install haskell 
ENV BOOTSTRAP_HASKELL_NONINTERACTIVE=1
ENV BOOTSTRAP_HASKELL_INSTALL_HLS=1
RUN curl --proto '=https' --tlsv1.2 -sSf https://get-ghcup.haskell.org | sh
ENV PATH=/root/.ghcup/bin/:$PATH

# Install Agda
RUN cabal install -j Agda
ENV PATH=/root/.cabal/bin:$PATH
RUN cd "$(agda --print-agda-data-dir)/lib/prim" \
    && find . -name "*.agda" -exec agda {} \;

# Install Agda stdlib
RUN wget -O agda-stdlib.tar.gz https://github.com/agda/agda-stdlib/archive/v2.2.tar.gz \
    && tar -xvzf agda-stdlib.tar.gz -C /usr/local/ \
    && mkdir -p $(agda  --print-agda-app-dir) \
    && echo "/usr/local/agda-stdlib-2.2/standard-library.agda-lib" > $(agda --print-agda-app-dir)/libraries \
    && rm -f agda-stdlib.tar.gz
