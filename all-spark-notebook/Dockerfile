# Copyright (c) Jupyter Development Team.
# Distributed under the terms of the Modified BSD License.
FROM sauloricci/notebook

MAINTAINER Saulo Ricci <infsaulok@gmail.com>

USER root

# RSpark config
ENV R_LIBS_USER $SPARK_HOME/R/lib

# R pre-requisites
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
    fonts-dejavu \
    gfortran \
    gcc && apt-get clean && \
    rm -rf /var/lib/apt/lists/*

USER $NB_USER

# R packages
RUN conda config --add channels r && \
    conda install --quiet --yes \
    'r-base=3.3.2' \
    'r-irkernel=0.7*' \
    'r-ggplot2=2.2*' \
    'r-rodbc=1.3_14' \
    'r-data.table=1.10.0' \
    'r-matrix=1.2_7.1' \
    'r-zoo=1.7_13' \ 
    'r-lubridate=1.6.0' \
    'r-sparklyr=0.5.1' \
    'r-dplyr=0.5.0' \
    'r-magrittr=1.5' \
    'r-rcurl=1.95*' && conda clean -tipsy

RUN conda install -c omnia 'arpack=3.2.0' && conda clean -tipsy

# Apache Toree kernel
RUN pip --no-cache-dir install https://dist.apache.org/repos/dist/dev/incubator/toree/0.2.0/snapshots/dev1/toree-pip/toree-0.2.0.dev1.tar.gz
RUN jupyter toree install --sys-prefix --spark_opts="--jars https://jdbc.postgresql.org/download/postgresql-42.0.0.jar --driver-class-path https://jdbc.postgresql.org/download/postgresql-42.0.0.jar" --spark_home=$SPARK_HOME

# Spylon-kernel
RUN pip --no-cache-dir install "spylon-kernel==0.1.2"
RUN python -m spylon_kernel install --sys-prefix
