FROM ubuntu:16.04
LABEL maintainer therecipe


RUN df -h


ENV USER user
ENV HOME /home/$USER
ENV QT_DOCKER true


RUN apt-get -qq update && apt-get -y -qq install curl git software-properties-common libgl1-mesa-dev fontconfig unzip && apt-get -qq clean
RUN apt-get -qq update && apt-get -y -qq install bison build-essential gperf flex ruby python libasound2-dev libbz2-dev libcap-dev libcups2-dev libdrm-dev libegl1-mesa-dev libgcrypt11-dev libnss3-dev libpci-dev libpulse-dev libudev-dev libxtst-dev gyp ninja-build && apt-get -qq clean
RUN apt-get -qq update && apt-get -y -qq install libssl-dev libxcursor-dev libxcomposite-dev libxdamage-dev libxrandr-dev libfontconfig1-dev libxss-dev libsrtp0-dev libwebp-dev libjsoncpp-dev libopus-dev libavutil-dev libavformat-dev libavcodec-dev libevent-dev libxslt1-dev && apt-get -qq clean


ENV GO go1.10.3.linux-amd64.tar.gz
RUN curl -sL --retry 10 --retry-delay 10 -o /tmp/$GO https://storage.googleapis.com/golang/$GO && tar -xzf /tmp/$GO -C /usr/local && rm -f /tmp/$GO
ENV PATH /usr/local/go/bin:$PATH
ENV GOPATH $HOME/work


RUN go get -v github.com/therecipe/qt/cmd/...
RUN go clean -i github.com/sirupsen/... && go clean -i golang.org/...
RUN rm -r $GOPATH/src/github.com/sirupsen/ && rm -r $GOPATH/src/golang.org/


ENV QT qt-unified-linux-x64-online.run
ENV QT_QPA_PLATFORM minimal
RUN curl -sL --retry 10 --retry-delay 10 -o /tmp/$QT https://download.qt.io/official_releases/online_installers/$QT && chmod +x /tmp/$QT && /tmp/$QT --script $GOPATH/src/github.com/therecipe/qt/internal/ci/iscript.qs LINUX=true && rm -f /tmp/$QT


ENV QT_DIR /opt/Qt
RUN $GOPATH/bin/qtsetup prep linux
RUN $GOPATH/bin/qtsetup check linux
RUN $GOPATH/bin/qtsetup generate linux
