FROM ubuntu:18.04
LABEL maintainer =  Mohamed Elashri <elashrmr@mail.uc.edu>

#Ignore generated .pyc files
ENV PYTHONDONTWRITEBYTECODE = 1.

# Python packages required in the build
COPY packages packages




# Install additional packages
RUN apt-get update -qq \
 && ln -sf /usr/share/zoneinfo/UTC /etc/localtime \
 && apt-get -y install $(cat packages) \
 && localedef -i en_US -f UTF-8 en_US.UTF-8 \
 && rm -rf /packages /var/lib/apt/lists/* \
 && python3 -m pip install --upgrade pip \
 && python3 -m pip install numpy scipy pandas matplotlib pypact \
 && mkdir software && cd software && git clone https://github.com/root-project/root \
 && mkdir root-build && cd root-build \
 && cmake -DBUILD_SHARED_LIBS=OFF \
        -DPYTHON_EXECUTABLE=/usr/bin/python3 \
        -Dasimage=ON \
        -Dcxx14=ON \
        -Dbuiltin_freetype=ON \
        -Dbuiltin_xrootd=ON \
        -Dbuiltin_tbb=ON \
        -Dbuiltin_vdt=ON \
        -Dgdml=ON \
        -Dopengl=OFF \
        -Dpythia6=OFF \
        -Dpythia8=ON \
        -Droofit=ON \
        -Doracle=OFF \
        -Dx11=ON \
        -Dshared=ON \
        -Droofit=ON \
        -Dfail-on-missing=ON \
        /software/root \
 && make -j16 \
 && make install -j16 \
 && cd /software && git clone https://github.com/Geant4/geant4 \
 && mkdir geant4-build && cd geant4-build \
 && cmake -DGEANT4_BUILD_MULTITHREADED=ON \
          -DGEANT4_BUILD_CXXSTD='14' \
          -DGEANT4_INSTALL_DATA=ON \
          -DGEANT4_USE_GDML=ON \
          -DGEANT4_USE_QT=ON \
          -DCMAKE_PREFIX_PATH=/usr/local/opt/qt/ \
          -DGEANT4_USE_OPENGL_X11=ON \
          -DGEANT4_USE_RAYTRACER_X11=ON \
          -DGEANT4_BUILD_VERBOSE_CODE=OFF \
          -DGEANT4_USE_PYTHON=ON \
          -DBUILD_STATIC_LIBS=ON \
          /software/geant4 \
 && make -j16 \
 && make install -j16 \
 && rm -rf /software

WORKDIR /

ENV ROOTSYS /usr/local
ENV G4INSTALLDIR /usr/local/

ENV PYTHONPATH ${ROOTSYS}/lib
ENV LD_LIBRARY_PATH ${ROOTSYS}/lib:${G4INSTALLDIR}/lib
ENV PATH="${PATH}:${ROOTSYS}"

ENV G4DATADIR ${G4INSTALLDIR}/share/Geant4-10.5.0/data
ENV G4NEUTRONHPDATA ${G4DATADIR}/G4NDL4.5
ENV G4LEDATA ${G4DATADIR}/G4EMLOW7.4
ENV G4LEVELGAMMADATA ${G4DATADIR}/PhotonEvaporation5.2
ENV G4RADIOACTIVEDATA ${G4DATADIR}/RadioactiveDecay5.2
ENV G4NEUTRONXSDATA ${G4DATADIR}/G4NEUTRONXS2.0
ENV G4PIIDATA ${G4DATADIR}/G4PII1.3
ENV G4REALSURFACEDATA ${G4DATADIR}/RealSurface2.1.1
ENV G4SAIDXSDATA ${G4DATADIR}/G4SAIDDATA1.1
ENV G4ABLADATA ${G4DATADIR}/G4ABLA3.1
ENV G4ENSDFSTATEDATA ${G4DATADIR}/G4ENSDFSTATE2.2

ENTRYPOINT /bin/bash -c "source ${ROOTSYS}/bin/thisroot.sh" && /bin/bash -c "source ${G4INSTALLDIR}/bin/geant4.sh" && /bin/bash
