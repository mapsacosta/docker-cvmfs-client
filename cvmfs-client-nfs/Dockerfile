FROM zonca/cvmfs-client:latest

COPY cvmfs_default.local /etc/cvmfs/default.local

RUN \
    yum -y --setopt=tsflags=nodocs install nfs-utils openssh-server openssh-clients rsync && \
    mkdir -p /exports && \
    yum clean all

RUN ssh-keygen -t rsa -f /etc/ssh/ssh_host_rsa_key -N ''

RUN  \
    mkdir -p /var/lib/nfs/rpc_pipefs /var/lib/nfs/v4recovery && \
    echo "rpc_pipefs    /var/lib/nfs/rpc_pipefs rpc_pipefs      defaults        0       0" >> /etc/fstab && \
    echo "nfsd  /proc/fs/nfsd   nfsd    defaults        0       0" >> /etc/fstab

EXPOSE 111/udp 2049/tcp

COPY exports /etc/
COPY nfsd.sh /usr/bin/nfsd.sh
RUN chmod +x /usr/bin/nfsd.sh

EXPOSE 22

ENTRYPOINT /usr/bin/nfsd.sh

#ENTRYPOINT mkdir -p /cvmfs/config-osg.opensciencegrid.org && \
#           mkdir -p /cvmfs/cdms.opensciencegrid.org && \
#           mkdir -p /cvmfs/sft.cern.ch && \
#           mkdir -p /cvmfs/geant4.cern.ch && \
#           mount -t cvmfs config-osg.opensciencegrid.org /cvmfs/config-osg.opensciencegrid.org && \
##           mount -t cvmfs cdms.opensciencegrid.org /cvmfs/cdms.opensciencegrid.org && \
#           mount -t cvmfs sft.cern.ch /cvmfs/sft.cern.ch && \
#           mount -t cvmfs geant4.cern.ch /cvmfs/geant4.cern.ch && \
#           /usr/bin/nfsd.sh
