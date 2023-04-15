FROM ubuntu:20.04 as apt-items

ENV DEBIAN_FRONTEND="noninteractive" TZ="America"

RUN apt-get update && apt-get install -y \
    apt-transport-https \
    curl \
    zip \
    unzip \
    jq \
    git \
    build-essential \
    sudo

FROM apt-items as aws-stage
RUN curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip" && \
    unzip awscliv2.zip && \
    sudo ./aws/install && \
    sudo rm -f awscliv2.zip

FROM aws-stage as tf-stage
RUN curl -O https://releases.hashicorp.com/terraform/1.2.8/terraform_1.2.8_linux_amd64.zip && \
    unzip terraform_1.2.8_linux_amd64.zip && \
    mv terraform /usr/local/bin/ && \
    rm terraform_1.2.8_linux_amd64.zip
