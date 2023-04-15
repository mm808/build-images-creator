FROM ubuntu:20.04 as apt-items

RUN apt-get update && apt-get install -y \
    curl \
    unzip \
    sudo \
    python-setuptools \
    python3-pip

FROM apt-items as aws-stage
RUN curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip" && \
    unzip awscliv2.zip && \
    sudo ./aws/install && \
    sudo rm -f awscliv2.zip

FROM aws-stage as pulumi-stage
RUN curl "https://get.pulumi.com/releases/sdk/pulumi-v3.35.1-linux-x64.tar.gz" -o "pulumi.tar.gz" && \
    tar -xf pulumi.tar.gz && \
    mv pulumi/* /usr/local/bin && \
    sudo chmod 777 /usr/local/bin/pulumi && \
    export PULUMI_SKIP_UPDATE_CHECK=true  && \
    sudo rm pulumi.tar.gz

FROM pulumi-stage as cleanup
RUN rm -rf /var/lib/apt/lists/* && \
    apt-get clean
