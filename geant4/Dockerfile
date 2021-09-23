FROM ubuntu:focal
LABEL maintainer =  Mohamed Elashri <elashrmr@mail.uc.edu>
ENV G4_version 10.07.p02

RUN apt-get update -y && \
    apt-get install -y cmake curl g++ libexpat1-dev vim cmake-curses-gui freeglut3 freeglut3-dev mesa-utils python libx11-dev libxmu-dev expat wget && \
    rm -rf /var/lib/apt/lists/* && \
    mkdir -p /opt/geant4/src && \
    mkdir -p /opt/geant4/build && \
    mkdir -p /opt/geant4/install && \
    mkdir -p /opt/geant4/data && \
    wget  https://geant4-data.web.cern.ch/releases/geant4.10.07.p02.tar.gz && \
    tar -xf /geant4.10.07.p02.tar.gz -C /opt/geant4/src && \
    cd /opt/geant4/build && \
    cmake -DCMAKE_INSTALL_PREFIX=/opt/geant4/install \
          -DGEANT4_USE_OPENGL_X11=ON \
          -DGEANT4_INSTALL_DATA=OFF \
          -DGEANT4_INSTALL_DATADIR=/opt/geant4/data \
          -DGEANT4_BUILD_MULTITHREADED=OFF \
          -DGEANT4_INSTALL_EXAMPLES=ON \
          ../src/geant4.10.07.p02 && \
    make -j`nproc` && \
    make install && \
    rm /geant4.tar.gz 

COPY entrypoint.sh /entrypoint.sh
ENTRYPOINT ["/entrypoint.sh"]
CMD ["/bin/bash"]

