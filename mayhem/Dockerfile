FROM --platform=linux/amd64 ubuntu:22.04 as builder

RUN apt-get -o Acquire::Retries=5 update
RUN DEBIAN_FRONTEND=noninteractive apt-get -o Acquire::Retries=5 install -y clang libjpeg-dev libz-dev build-essential

RUN apt-get -o Acquire::Retries=5 install -y apt-transport-https curl gnupg
RUN curl -fsSL https://bazel.build/bazel-release.pub.gpg | gpg --dearmor > bazel.gpg
RUN mv bazel.gpg /etc/apt/trusted.gpg.d/
RUN echo 'deb [arch=amd64] https://storage.googleapis.com/bazel-apt stable jdk1.8' | tee /etc/apt/sources.list.d/bazel.list
RUN apt-get -o Acquire::Retries=5 update && apt-get -o Acquire::Retries=5 install -y bazel-4.0.0

ENV CC=clang
ENV CXX=clang++

COPY . /repo
WORKDIR /repo
RUN DEBIAN_FRONTEND=noninteractive apt-get -o Acquire::Retries=5 install -y git
RUN bazel-4.0.0 build //programl/bin:graph2json

FROM ubuntu:22.04 as package
COPY --from=builder /repo/bazel-bin/programl/bin/graph2json /
