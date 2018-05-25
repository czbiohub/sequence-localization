
# These are the CUDA and CUDAnn versions on fry
ARG cuda_version=8.0
ARG cudnn_version=6
FROM nvidia/cuda:${cuda_version}-cudnn${cudnn_version}-devel

# Install system packages
RUN apt-get update && apt-get install -y --no-install-recommends \
    bzip2 \
    g++ \
    git \
    graphviz \
    libgl1-mesa-glx \
    libhdf5-dev \
    openmpi-bin \
    wget && \
    rm -rf /var/lib/apt/lists/*

# Install conda
ENV CONDA_DIR /opt/conda
ENV PATH $CONDA_DIR/bin:$PATH

RUN wget --quiet --no-check-certificate https://repo.continuum.io/miniconda/Miniconda3-4.2.12-Linux-x86_64.sh && \
    echo "c59b3dd3cad550ac7596e0d599b91e75d88826db132e4146030ef471bb434e9a *Miniconda3-4.2.12-Linux-x86_64.sh" | sha256sum -c - && \
        /bin/bash /Miniconda3-4.2.12-Linux-x86_64.sh -f -b -p $CONDA_DIR && \
	    rm Miniconda3-4.2.12-Linux-x86_64.sh && \
	        echo export PATH=$CONDA_DIR/bin:'$PATH' > /etc/profile.d/conda.sh

# Install Python packages and keras
ENV NB_USER keras
ENV NB_UID 1000

RUN useradd -m -s /bin/bash -N -u $NB_UID $NB_USER && \
    chown $NB_USER $CONDA_DIR -R && \
        mkdir -p /src && \
	    chown $NB_USER /src

USER $NB_USER

ARG python_version=3.6

RUN conda install -y python=${python_version} && \
    pip install --upgrade pip && \
    pip install \
    sklearn_pandas && \
    pip install https://cntk.ai/PythonWheel/GPU/cntk-2.1-cp36-cp36m-linux_x86_64.whl && \
    conda install \
    bcolz \
    h5py \
    matplotlib \
    mkl \
    nose \
    notebook \
    Pillow \
    pandas \
    pygpu \
    pyyaml \
    scikit-learn \
    six \
    # keras-genomics specific
    biopython \
    networkx \
    pymongo \
    theano  && \
    git clone git://github.com/keras-team/keras.git /src && pip install -e /src[tests] && \
    pip install git+git://github.com/keras-team/keras.git && \
    conda install tensorflow-gpu && \
    conda clean -yt

#ADD theanorc /home/keras/.theanorc
ENV PYTHONPATH='/src/:$PYTHONPATH'

WORKDIR /src

EXPOSE 8888

CMD jupyter notebook --port=8888 --ip=0.0.0.0



# Keras-genomcis specific
RUN pip install --upgrade --no-deps git+https://github.com/maxpumperla/hyperas
RUN pip install --upgrade --no-deps git+https://github.com/hyperopt/hyperopt.git


ENV THEANO_FLAGS='cuda.root=/usr/local/cuda,device=gpu0,floatX=float32,lib.cnmem=0.1,base_compiledir=/runtheano/.theano'

RUN git clone https://github.com/gifford-lab/keras-genomics.git /src/keras-genomics
RUN cp /src/keras-genomics/main.py /src/

USER root
RUN mkdir /runtheano/; chmod -R 777 /runtheano/

