FROM debian:buster-slim

LABEL name="drivmarkets" version=$VERSION

ARG VCS_REF
ARG BUILD_DATE

LABEL org.label-schema.vcs-ref=$VCS_REF \
      org.label-schema.vcs-url="https://github.com/$USER/vcs.git"

RUN export DEBIAN_FRONTEND=noninteractive && \
    apt-get update && \
    apt update && \
		apt-get upgrade -y && \
    apt-get install -y tzdata software-properties-common git vim wget libssl-dev && \
    apt-get autoremove --purge \
    rm -rf /var/lib/apt/lists/* \
    apt-get -y clean


# Miniconda
RUN echo 'export PATH=/opt/conda/bin:$PATH' >> /root/.bashrc && \
    wget --quiet https://repo.continuum.io/miniconda/Miniconda3-latest-Linux-x86_64.sh -O ~/miniconda.sh && \
    /bin/bash ~/miniconda.sh -b -p /opt/conda && \
    rm ~/miniconda.sh && \
    rm -rf /var/lib/apt/lists/*

ENV PATH /opt/conda/bin:$PATH

RUN wget https://raw.githubusercontent.com/jeffheaton/t81_558_deep_learning/master/tensorflow.yml && \
    conda install pip jupyter && \
    conda env create -v -f tensorflow.yml && \ 
    conda init bash && \
    rm -rf /var/lib/apt/lists/*

# R pre-requisites
RUN apt-get update && \
    apt-get install -y --no-install-recommends fonts-dejavu gfortran \
    gcc && apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# R
#RUN conda install -c r ipython-notebook r-irkernel
RUN apt-get update && \
    apt-get install -y r-base && \
    conda install -c r r-irkernel r-essentials -c conda-forge && \
		apt-get autoremove --purge \
    rm -rf /var/lib/apt/lists/*

COPY packages.r /root/packages.r

RUN ln -s /bin/tar /bin/gtar && \
    Rscript /root/packages.r

WORKDIR /root/

EXPOSE 8888

CMD ["sh", "-c", "jupyter notebook --ip=0.0.0.0 --allow-root"]
