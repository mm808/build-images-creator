FROM ubuntu:22.04 as apt-items

ENV DEBIAN_FRONTEND="noninteractive" TZ="America" UBUNTU_CODENAME=jammy

RUN apt-get update && apt-get install -y apt-transport-https curl zip unzip \
    jq git build-essential sudo nano

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

RUN export DEBIAN_FRONTEND=noninteractive && \
    echo 'debconf debconf/frontend select Noninteractive' | debconf-set-selections && \
    echo 'tzdata tzdata/Areas select America' | debconf-set-selections && \
    echo 'tzdata tzdata/Zones/America select New_York' | debconf-set-selections && \
    curl -o - "https://keyserver.ubuntu.com/pks/lookup?fingerprint=on&op=get&search=0x6125E2A8C77F2818FB7BD15B93C4A3FD7BB9C367" | \
    sudo gpg --dearmour -o /usr/share/keyrings/ansible-archive-keyring.gpg && \
    echo "deb [signed-by=/usr/share/keyrings/ansible-archive-keyring.gpg] http://ppa.launchpad.net/ansible/ansible/ubuntu \
    $UBUNTU_CODENAME main" | sudo tee /etc/apt/sources.list.d/ansible.list && \
    sudo apt update && sudo apt install ansible -y
