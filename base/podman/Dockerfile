FROM rockylinux:9.3

# utils
RUN yum install -y epel-release ca-certificates && \
  yum install -y unzip \
  which \
  make \
  wget \
  zip \
  bzip2 \
  gcc \
  gcc-c++ \
  curl-devel \
  autoconf \
  expat-devel \
  gettext-devel \
  openssl-devel \
  perl-devel \
  zlib-devel \
  python-pip \
  java-1.8.0-openjdk telnet git  tzdata && \
  ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime && \
  echo "Asia/Shanghai" > /etc/timezone && \
  yum -y clean all --enablerepo='*'

# Set the locale(en_US.UTF-8)
ENV LANG en_US.UTF-8
ENV LANGUAGE en_US:en
ENV LC_ALL en_US.UTF-8

# USER jenkins
WORKDIR /home/jenkins

ENV SONAR_SCANNER_VERSION 3.3.0.1492

RUN curl -o sonar_scanner.zip https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-${SONAR_SCANNER_VERSION}-linux.zip && \
    unzip sonar_scanner.zip && rm sonar_scanner.zip \
    && rm -rf sonar-scanner-$SONAR_SCANNER_VERSION-linux/jre && \
    sed -i 's/use_embedded_jre=true/use_embedded_jre=false/g' /home/jenkins/sonar-scanner-$SONAR_SCANNER_VERSION-linux/bin/sonar-scanner && \
    mv /home/jenkins/sonar-scanner-$SONAR_SCANNER_VERSION-linux /usr/bin

ENV PATH $PATH:/usr/bin/sonar-scanner-$SONAR_SCANNER_VERSION-linux/bin

COPY ./ ./

ENV EXCLUDE_DOCKER 1
RUN ./hack/install_utils.sh && rm -rf ./*

# Install podman
RUN yum -y install podman fuse-overlayfs && \
    ln -s /usr/bin/podman /usr/bin/docker && \
    yum -y clean all --enablerepo='*'

COPY storage.conf /etc/containers/storage.conf
COPY containers.conf /etc/containers/containers.conf

VOLUME /var/lib/containers

CMD ["podman", "info"]
