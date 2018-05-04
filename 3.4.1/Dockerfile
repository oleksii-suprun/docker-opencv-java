FROM frolvlad/alpine-oraclejre8

LABEL maintainer="Aleksey Suprun <alexey.suprun@gmail.com>"

ENV LANG C.UTF-8

ENV OPENCV_VERSION 3.4.1
ENV OPENCV_SRC_URL "https://github.com/opencv/opencv/archive/$OPENCV_VERSION.tar.gz"
ENV OPENCV_INSTALL_PREFIX "/usr/local/opencv"
ENV OPENCV_JAVA_PATH "$OPENCV_INSTALL_PREFIX/share/OpenCV/java"

RUN set -x \
  && apk add --no-cache libstdc++

RUN set -x \
    && apk add --no-cache --virtual .build-dependencies \
        cmake \
        tar \
        ca-certificates \
        make \
        g++ \
        linux-headers \
        python \
    && mkdir -p /tmp/jdk && cd /tmp/jdk \
    && wget \
        --header "Cookie: oraclelicense=accept-securebackup-cookie;" \
        -O jdk.tar.gz \
        "http://download.oracle.com/otn-pub/java/jdk/8u172-b11/a58eab1ec242421181065cdc37240b08/jdk-8u172-linux-x64.tar.gz" \
    && tar -xzf jdk.tar.gz --strip-components=1 \
    && export JAVA_HOME=/tmp/jdk \
    && export PATH=$PATH:$JAVA_HOME/bin \
    && mkdir -p /tmp/ant && cd /tmp/ant \
    && wget -O apache-ant.tar.gz "http://archive.apache.org/dist/ant/binaries/apache-ant-1.10.1-bin.tar.gz" \
    && tar -xzf apache-ant.tar.gz --strip-components=1 \
    && export ANT_HOME=/tmp/ant \
    && export PATH=$PATH:$ANT_HOME/bin \
    && mkdir -p /tmp/opencv && cd /tmp/opencv \
    && wget -O opencv-src.tar.gz "$OPENCV_SRC_URL" \
    && tar -xzf opencv-src.tar.gz --strip-components=1 \
    && mkdir build && cd build \
    && cmake \
        -D CMAKE_BUILD_TYPE=Release \
        -D CMAKE_INSTALL_PREFIX="$OPENCV_INSTALL_PREFIX" \
        -D BUILD_SHARED_LIBS=OFF \
        -D BUILD_EXAMPLES=OFF \
        -D BUILD_TESTS=OFF \
        -D BUILD_PERF_TESTS=OFF \
        -D BUILD_JAVA=ON \
        .. \
    && echo $JAVA_HOME \
    && echo $PATH \
    && make -j$(nproc) \
    && make install \
    && cd / \
    && apk del .build-dependencies \
    && rm -r /tmp/*

CMD /bin/sh
