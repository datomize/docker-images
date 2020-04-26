# Original Anaconda dockerfile
# Change to Ubuntu so know all the tools work
FROM ubuntu:bionic

ENV LANG=C.UTF-8 LC_ALL=C.UTF-8
ENV PATH /opt/conda/bin:$PATH

RUN apt-get update --fix-missing && \
    apt-get install -y wget curl bzip2 ca-certificates libglib2.0-0 libxext6 libsm6 libxrender1 procps git  && \
    apt-get clean
#     apt-get install -y wget bzip2 ca-certificates libglib2.0-0 libxext6 libsm6 libxrender1 procps git mercurial subversion -- got rid of last two and added procps
RUN wget --quiet https://repo.anaconda.com/archive/Anaconda3-2020.02-Linux-x86_64.sh -O ~/anaconda.sh && \
    /bin/bash ~/anaconda.sh -b -p /opt/conda && \
    rm ~/anaconda.sh && \
    ln -s /opt/conda/etc/profile.d/conda.sh /etc/profile.d/conda.sh && \
    echo ". /opt/conda/etc/profile.d/conda.sh" >> ~/.bashrc && \
    find /opt/conda/ -follow -type f -name '*.a' -delete && \
    find /opt/conda/ -follow -type f -name '*.js.map' -delete && \
    /opt/conda/bin/conda clean -afy

# Set up conda for ongoing use
RUN     /opt/conda/bin/conda init bash

RUN /opt/conda/bin/conda update conda -y && \
    /opt/conda/bin/conda update --all -y && \
    /opt/conda/bin/conda create --name jupyter --clone base  -y && \
    echo "conda activate jupyter" >> ~/.bashrc && \
    mkdir /opt/notebooks

# prepare for postgres access
RUN /opt/conda/bin/conda install -c conda-forge jupyter_nbextensions_configurator jupyter_contrib_nbextensions fs.sshfs -y && \
    /opt/conda/bin/conda install psycopg2

CMD [ "/bin/bash" ]

# Build this image: docker build --tag jupyter:v1 .
# Run this image to test: docker run -p 8888:8888 jupyter:v1  /bin/bash -c "/opt/conda/bin/jupyter lab --notebook-dir=/opt/notebooks --ip='*' --port=8888 --no-browser --allow-root"

