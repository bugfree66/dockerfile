# phrase build dig
FROM archlinux/base
RUN    pacman --noconfirm -Syu \
    && pacman --noconfirm -S gcc make glibc gdb man-pages \
    && pacman --noconfirm -S git python python-pip awk diffutils
RUN    git config --global user.email "lichang.eric@gmail.com" \
    && git config --global user.name "Eric Lee" \
    && git config --global http.postBuffer 524288000    
RUN    cd /root \
    && git clone https://gitlab.isc.org/isc-projects/bind9.git \
    && cd bind9 \
    && pip install ply \
    && ./configure --prefix=/root/bind9/output \
    && make && make install   
    
# phrase build image
FROM archlinux/base

LABEL maintainer="Eric Lee <lichang.eric@gmail.com>"

RUN    echo "zh_CN.UTF-8 UTF-8" >> /etc/locale.gen \
    && echo "en_US.UTF-8 UTF-8" >> /etc/locale.gen \
    && locale-gen

ENV LANG en_US.UTF-8

VOLUME /data

RUN    pacman --noconfirm -Syu \
    && pacman --noconfirm -S gcc make glibc gdb man-pages \
    && pacman --noconfirm -S vim ctags cscope \
    && pacman --noconfirm -S llvm lld clang cmake \
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
#RUN    npm install hexo-cli -g
#RUN    cd /root \
#    && git clone https://github.com/kitrap/kitrap.git \
#    && cd kitrap \
#    && npm install hexo --save \
#    && npm install hexo-deployer-git --save \
#    && npm cache clean --force
    
# bind9
#RUN    cd /root \
#    && git clone https://gitlab.isc.org/isc-projects/bind9.git \
#    && cd bind9 \
#    && ./configure \
#    && make && make install \
#    && cd .. \
#    && rm -rf bind9
COPY --from=0 /root/bind9/output/bin/dig /usr/local/bin/
COPY --from=0 /root/bind9/output/bin/nslookup /usr/local/bin/

# config vim
RUN    cd /root \
    && git clone https://github.com/lc7cl/MyVIM.git \
    && cp MyVIM/.vimrc* /root/ \
    && mkdir -p /root/.vim/bundle \
    && git clone https://github.com/VundleVim/Vundle.vim.git /root/.vim/bundle/vundle \
#    && echo | echo | vim +PluginInstall +qall &>/dev/null \
    && git clone https://github.com/jdevera/parallel-vundle-installer.git \
    && cd parallel-vundle-installer \
    && ./pvi -j 32 -b "/root/.vim/bundles" > /dev/null \
    && rm -rf parallel-vundle-installer 
#    && vim -E --not-a-term -S /root/.vimrc.bundles +PluginInstall +qall > /dev/null \
#    && vim -E -u NONE -S /root/.vimrc.bundles +PluginInstall +qall > /dev/null \
#    && mkdir -p /root/.vim/snippets \
#    && cd /root/.vim/bundle/YouCompleteMe \
#    && git submodule update --init --recursive \
#    && ./install.py --clang-completer \
#    && ./install.py --go-completer \
#    && ./install.py --java-completer \
#    && ./install.py --js-completer 

CMD ["/bin/bash"]
