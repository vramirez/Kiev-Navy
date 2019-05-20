FROM ubuntu

USER root

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


# Configure environment
ENV CONDA_DIR=/opt/conda \
    SHELL=/bin/bash \
    NB_USER=vramirez \
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

RUN git clone https://github.com/fastai/fastai \
    && cd fastai \
    && conda create -n fastai python=3.6 anaconda \
    && conda env update 

EXPOSE 8080
EXPOSE 8443
