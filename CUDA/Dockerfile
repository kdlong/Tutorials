FROM nvidia/cuda:12.6.3-cudnn-devel-ubuntu20.04

RUN apt-get update && apt-get install -y build-essential \
    && apt-get install -y wget  && apt-get clean

RUN apt-get install vim -y

# Needed to replace nvprof with nsys
# From https://forums.developer.nvidia.com/t/installing-nsight-systems-in-docker-container/294376/5
ARG NSYS_URL=https://developer.nvidia.com/downloads/assets/tools/secure/nsight-systems/2024_4/
ARG NSYS_PKG=NsightSystems-linux-cli-public-2024.4.1.61-3431596.deb

ENV TZ=Europe/Zurich \
    DEBIAN_FRONTEND=noninteractive

RUN apt-get update && apt install -y wget libglib2.0-0
RUN wget ${NSYS_URL}${NSYS_PKG} && dpkg -i $NSYS_PKG && rm $NSYS_PKG
