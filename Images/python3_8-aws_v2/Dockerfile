FROM ubuntu:20.04 as apt-items

RUN apt-get update && apt-get install -y \
    curl \
    zip \
    unzip \
    sudo \
    python-setuptools \
    python3-pip

FROM apt-items as aws-stage
RUN curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip" && \
    unzip awscliv2.zip && \
    sudo ./aws/install && \
    sudo rm -f awscliv2.zip

FROM aws-stage as clean-stage
RUN rm -rf /var/lib/apt/lists/* && \
    apt-get clean
