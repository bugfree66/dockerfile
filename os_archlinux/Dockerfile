
FROM base/archlinux

LABEL maintainer="Eric Lee <lichang.eric@gmail.com>"

RUN    echo "zh_CN.UTF-8 UTF-8" >> /etc/locale.gen \
    && echo "en_US.UTF-8 UTF-8" >> /etc/locale.gen \
    && locale-gen

ENV LANG en_US.UTF-8

VOLUME /data

RUN    pacman --noconfirm -Syu \
    && pacman --noconfirm -S gcc make glibc gdb man-pages \
    && pacman --noconfirm -S vim ctags cscope \
    && pacman --noconfirm -S llvm lld clang \
    && pacman --noconfirm -S go boost python \
    && pacman --noconfirm -S iproute2 git nodejs npm \
    && pacman --noconfirm -S scp \
    && pacman --noconfirm -S python-pip \
    && rm -f \
      /var/cache/pacman/pkg/* \
      /var/lib/pacman/sync/* \
      /etc/pacman.d/mirrorlist.pacnew

RUN    git config --global user.email "lichang.eric@gmail.com" \
    && git config --global user.name "Eric Lee" \
    && git config --global http.postBuffer 524288000
      
RUN    npm config set registry https://registry.npm.taobao.org

# config pip 
RUN    mkdir /root/.pip
RUN    echo "[global]" >> /root/.pip/pip.conf \
    && echo "trusted-host = mirrors.aliyun.com" >> /root/.pip/pip.conf \
    && echo "index-url = http://mirrors.aliyun.com/pypi/simple" >> /root/.pip/pip.conf

# hexo & blog
RUN    npm install hexo-cli -g
RUN    cd /root \
    && git clone https://github.com/kitrap/kitrap.git \
    && cd kitrap \
    && npm install hexo --save \
    && npm install hexo-deployer-git --save \
    && npm cache clean --force
    
# bind9
RUN    cd /root \
    && git clone https://gitlab.isc.org/isc-projects/bind9.git \
    && cd bind9 \
    && ./configure \
    && make && make install \
    && cd .. \
    && rm -rf bind9

# config vim
RUN    cd /root \
    && git clone https://github.com/lc7cl/MyVIM.git \
    && cp MyVIM/.vimrc /root/ \
    && mkdir -p /root/.vim/bundle \
    && git clone https://github.com/gmarik/Vundle.vim.git  /root/.vim/bundle/Vundle.vim
RUN    echo | echo | vim +PluginInstall +qall &>/dev/null \
    && mkdir -p /root/.vim/snippets

CMD ["/bin/bash"]