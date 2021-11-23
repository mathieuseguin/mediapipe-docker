FROM ubuntu:latest

ARG DEBIAN_FRONTEND=noninteractive 

RUN apt update \
    && apt install -y \
      cmake \
      git \
      protobuf-compiler \
      python3-dev \
      python3-pip \
      python3.8-venv \
      sudo \
      unzip \
      vim \
      wget

ENV BAZEL_VERSION=3.7.2

RUN mkdir ./bazel \
    && wget --no-check-certificate -O ./bazel/installer.sh "https://github.com/bazelbuild/bazel/releases/download/${BAZEL_VERSION}/bazel-${BAZEL_VERSION}-installer-linux-x86_64.sh" \
    && wget --no-check-certificate -O  ./bazel/LICENSE.txt "https://raw.githubusercontent.com/bazelbuild/bazel/master/LICENSE" \
    && chmod +x ./bazel/installer.sh \
    && ./bazel/installer.sh \
    && rm -f ./bazel/installer.sh

WORKDIR /usr/local/src

RUN git clone https://github.com/google/mediapipe.git

WORKDIR /usr/local/src/mediapipe

COPY ./setup_opencv.patch /tmp/setup_opencv.patch

RUN patch setup_opencv.sh /tmp/setup_opencv.patch

RUN bash setup_opencv.sh

RUN python3 -m venv mp_env \
    && . mp_env/bin/activate \
    && pip3 install -r requirements.txt \
    && pip3 install pip setuptools --upgrade \
    && pip3 install --upgrade pip wheel \
    && python3 setup.py gen_protos \
    && python3 setup.py install