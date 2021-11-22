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

RUN apt-get update \
    && apt-get install -y --no-install-recommends \
      libgtk2.0-dev \
      libavcodec-dev \
      libavformat-dev \
      libswscale-dev \
      libtbb2 \
      libv4l-dev \
      libtbb-dev \
      libjpeg-dev \
      libpng-dev \
      libtiff-dev \
      libdc1394-22-dev \
      gstreamer1.0-alsa \
      gstreamer1.0-libav \
      gstreamer1.0-nice \
      gstreamer1.0-plugins-bad \
      gstreamer1.0-plugins-base \
      gstreamer1.0-plugins-base-apps \
      gstreamer1.0-plugins-good \
      gstreamer1.0-plugins-rtp \
      gstreamer1.0-plugins-ugly \
      gstreamer1.0-python3-plugin-loader \
      gstreamer1.0-rtsp \
      gstreamer1.0-tools \
      libgstreamer1.0-0 \
      libgstreamer1.0-dev \
      libgstreamer-plugins-bad1.0-0 \
      libgstreamer-plugins-bad1.0-dev \
      libgstreamer-plugins-base1.0-0 \
      libgstreamer-plugins-base1.0-dev \
      libgstreamer-plugins-good1.0-0 \
      libgstreamer-plugins-good1.0-dev

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
COPY ./opencv_linux.BUILD.patch /tmp/opencv_linux.BUILD.patch

RUN patch setup_opencv.sh /tmp/setup_opencv.patch
RUN patch third_party/opencv_linux.BUILD /tmp/opencv_linux.BUILD.patch

RUN bash setup_opencv.sh

RUN python3 -m venv mp_env \
    && . mp_env/bin/activate \
    && pip3 install -r requirements.txt \
    && pip3 install pip setuptools --upgrade \
    && pip3 install --upgrade pip wheel \
    && python3 setup.py gen_protos \
    && python3 setup.py bdist_wheel