
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
    && rm -f \
      /var/cache/pacman/pkg/* \
      /var/lib/pacman/sync/* \
      /etc/pacman.d/mirrorlist.pacnew

RUN    git config --global user.email "lichang.eric@gmail.com" \
    && git config --global user.name "Eric Lee" \
    && git config --global http.postBuffer 524288000
      
RUN    npm config set registry https://registry.npm.taobao.org

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

CMD ["/bin/bash"]
