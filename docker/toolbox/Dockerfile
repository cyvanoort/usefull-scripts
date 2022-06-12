FROM registry.hub.docker.com/library/debian:bullseye

#ARGS
ARG user_name=cyvan
ARG user_id=501
ARG user_home=/home/$userName

#Environment settings
ENV USERNAME=$user_name
ENV USERID=$user_id
ENV USERHOME=$user_home

RUN apt-get update && \
    DEBIAN_FRONTEND=noninteractive apt-get install -y \
        build-essential \
        cmake \
        curl \
        git \
        jq \
        libncurses-dev \
        libncurses6 \
        python3 \
        python3-dev \
        vim \
        nmap \
        screen \
        sudo && \
   echo '%sudo	ALL=(ALL) NOPASSWD: ALL' > /etc/sudoers.d/root

# Create a local user matching the system user for toolbox style integration
RUN /usr/sbin/useradd -u "$USERID"                       \
                      -U -Gsudo -d "$USERHOME"           \
                      -s /bin/bash "$USERNAME" 

# Install Golang
ENV GOLANG_VERSION 1.18.1
ENV GOLANG_SHA b3b815f47ababac13810fc6021eb73d65478e0b2db4b09d348eefad9581a2334
RUN curl -L https://golang.org/dl/go${GOLANG_VERSION}.linux-amd64.tar.gz -o go.tar.gz && \
  echo "${GOLANG_SHA}  go.tar.gz" | sha256sum -c && \
  tar -C /usr/local -xzf go.tar.gz && \
  mkdir /go && \
  chown -R "${USERID}:${USERID}" /go && \
  rm go.tar.gz
ENV GOPATH /go
RUN ln -s /usr/local/go/bin/go /usr/bin/

# Install kubectl
ENV K8S_VERSION v1.23.6
ENV K8S_SHA 703a06354bab9f45c80102abff89f1a62cbc2c6d80678fd3973a014acc7c500a
RUN curl -L https://dl.k8s.io/release/${K8S_VERSION}/bin/linux/amd64/kubectl -o kubectl && \
  echo "${K8S_SHA} kubectl" | sha256sum -c && \
  chmod +x kubectl && \
  mkdir -p /toolbox/bin && \
  mv ./kubectl /toolbox/bin/kubectl


ENV PATH /toolbox/bin/:$GOPATH/bin:$PATH
USER $USERNAME
