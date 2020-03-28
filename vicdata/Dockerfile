FROM ubuntu

USER root

#Install basic commands like wget and sudo

RUN apt-get update && apt-get -yq dist-upgrade \
 && apt-get install -yq --no-install-recommends \
    wget \
    bash \
    #bzip2 \
    curl \
    ca-certificates \
    sudo \
    #locales \
    #fonts-liberation \
    #build-essential \
    #file \
     git \
 && apt-get clean \
 && rm -rf /var/lib/apt/lists/*


# Configure environment
ENV SHELL=/bin/bash \
    NB_USER=vramirez \
    NB_UID=1000 \
    NB_GID=100 \
    LC_ALL=en_US.UTF-8 \
    LANG=en_US.UTF-8 \
    LANGUAGE=en_US.UTF-8
ENV PATH=$CONDA_DIR/bin:$PATH \
    HOME=/home/$NB_USER \
    CONDA_ENV=ashgabat 

ENV CONDA_DIR=$HOME/miniconda3 

RUN useradd -s ${SHELL} -m ${NB_USER} 
##Install conda (& Python)
USER ${NB_USER}

RUN curl -sSL https://repo.continuum.io/miniconda/Miniconda3-latest-Linux-x86_64.sh -o /tmp/miniconda.sh \
    && bash /tmp/miniconda.sh -b -p ${CONDA_DIR} \
    && export PATH="$CONDA_DIR/bin:$PATH" \
    && rm -rf /tmp/miniconda.sh \
    && conda install -y python=3 \
    && conda update conda \
    #&& apt-get -qq -y autoremove \
    #&& apt-get autoclean \
    #For sudoers
    #&& rm -rf /var/lib/apt/lists/* /var/log/dpkg.log \
    && conda clean --all --yes \
    && conda create -n ${CONDA_ENV} python=3  
    #&& conda env update \
    #&& conda init bash \
    #&& conda activate ${CONDA_ENV}



RUN cd ${HOME} \
&& git clone https://github.com/vramirez/Ashgabat-Black.git 



USER root

RUN sudo  apt-get -qq -y autoremove \
    && apt-get autoclean \
    #For sudoers
    && rm -rf /var/lib/apt/lists/* /var/log/dpkg.log 

USER ${NB_USER}
RUN cd ${HOME}
#RUN source $CONDA_DIR/bin/activate
