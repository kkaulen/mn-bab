FROM nvidia/cuda:12.4.1-devel-ubuntu20.04


RUN apt-get update
RUN apt-get -y install git build-essential m4 perl wget sudo
RUN apt-get update
RUN apt-get -y install python3.8 python3-pip

RUN mkdir ./app
WORKDIR ./app

COPY . ./mn-bab

WORKDIR ./mn-bab

RUN ["/bin/bash", "-c", "source ./setup.sh"]

RUN ["pip3", "install", "-r", "requirements.txt"]

ENV PYTHONPATH /app/mn-bab/:/app/mn-bab/ELINA/python_interface
ENV LD_LIBRARY_PATH $LD_LIBRARY_PATH:/usr/local/lib

RUN git submodule init
RUN git submodule sync
RUN git submodule set-url dnnv https://github.com/dlshriver/dnnv.git
RUN git submodule set-url onnx2pytorch https://github.com/mnmueller/onnx2pytorch_vnn.git
RUN git submodule update

RUN touch ./dnnv/__init__.py
RUN mkdir ./logs
