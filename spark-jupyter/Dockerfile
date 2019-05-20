FROM ubuntu

USER root

ENV APACHE_SPARK_VERSION 2.4.0
ENV HADOOP_VERSION 2.7
ENV ZEPPELIN_VERSION 0.8.0
#Install basic commands like wget and sudo

RUN apt-get update && apt-get -yq dist-upgrade \
 && apt-get install -yq --no-install-recommends \
    wget \
    bzip2 \
    curl \
    ca-certificates \
    sudo \
    locales \
    fonts-liberation \
    build-essential \
    file \
     git \
 && apt-get clean \
 && rm -rf /var/lib/apt/lists/*


RUN apt-get -y update && \
    apt-get install --no-install-recommends -y openjdk-8-jre-headless ca-certificates-java && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

RUN cd /tmp && \
        wget -q http://mirrors.ukfast.co.uk/sites/ftp.apache.org/spark/spark-${APACHE_SPARK_VERSION}/spark-${APACHE_SPARK_VERSION}-bin-hadoop${HADOOP_VERSION}.tgz && \
        echo "DC3A97F3D99791D363E4F70A622B84D6E313BD852F6FDBC777D31EAB44CBC112CEEAA20F7BF835492FB654F48AE57E9969F93D3B0E6EC92076D1C5E1B40B4696 *spark-${APACHE_SPARK_VERSION}-bin-hadoop${HADOOP_VERSION}.tgz" | sha512sum -c - && \
        tar xzf spark-${APACHE_SPARK_VERSION}-bin-hadoop${HADOOP_VERSION}.tgz -C /usr/local --owner root --group root --no-same-owner && \
        rm spark-${APACHE_SPARK_VERSION}-bin-hadoop${HADOOP_VERSION}.tgz

RUN cd /usr/local && ln -s spark-${APACHE_SPARK_VERSION}-bin-hadoop${HADOOP_VERSION} spark

ENV SPARK_HOME /usr/local/spark
ENV SPARK_OPTS --driver-java-options=-Xms1024M --driver-java-options=-Xmx4096M --driver-java-options=-Dlog4j.logLevel=info
ENV PYTHONPATH $SPARK_HOME/python

# Configure environment
ENV CONDA_DIR=/opt/conda \
    SHELL=/bin/bash \
    NB_USER=jovyan \
    NB_UID=1000 \
    NB_GID=100 \
    LC_ALL=en_US.UTF-8 \
    LANG=en_US.UTF-8 \
    LANGUAGE=en_US.UTF-8
ENV PATH=$CONDA_DIR/bin:$PATH \
    HOME=/home/$NB_USER


##Install conda (& Python)

RUN curl -sSL https://repo.continuum.io/miniconda/Miniconda3-latest-Linux-x86_64.sh -o /tmp/miniconda.sh \
    && bash /tmp/miniconda.sh -bfp /usr/local \
    && rm -rf /tmp/miniconda.sh \
    && conda install -y python=3 \
    && conda update conda \
    && apt-get -qq -y autoremove \
    && apt-get autoclean \
    && rm -rf /var/lib/apt/lists/* /var/log/dpkg.log \
    && conda clean --all --yes

#Download and Install Zeppelin
RUN wget -q http://www-us.apache.org/dist/zeppelin/zeppelin-${ZEPPELIN_VERSION}/zeppelin-${ZEPPELIN_VERSION}-bin-netinst.tgz \
    && tar xzf zeppelin-${ZEPPELIN_VERSION}-bin-netinst.tgz -C /usr/local --owner root --group root --no-same-owner \
    && cd /usr/local && ln -s zeppelin-${ZEPPELIN_VERSION}-bin-netinst zeppelin \
    && cd
#   && rm zeppelin-${ZEPPELIN_VERSION}-bin-netinst.tgz

EXPOSE 8080
EXPOSE 8443
