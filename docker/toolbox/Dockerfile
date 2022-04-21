FROM registry.hub.docker.com/library/debian:bullseye

#ARGS
ARG userName=cyvan
ARG userId=1000
ARG userHome=/home/$userName

#Environment settings
ENV USERNAME=$userName
ENV USERID=$userId
ENV USERHOME=$userHome

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
        sudo && \
   echo '%sudo	ALL=(ALL) NOPASSWD: ALL' > /etc/sudoers.d/root

# Create a local user matching the system user for toolbox style integration
RUN /usr/sbin/useradd -u "$USERID"                       \
                      -U -Gsudo -d "$USERHOME"           \
                      -s /bin/bash "$USERNAME" 

# Install Golang
ENV GOLANG_VERSION 1.17.7
ENV GOLANG_SHA 02b111284bedbfa35a7e5b74a06082d18632eff824fd144312f6063943d49259
RUN curl -L https://golang.org/dl/go${GOLANG_VERSION}.linux-amd64.tar.gz -o go.tar.gz && \
  echo "${GOLANG_SHA}  go.tar.gz" | sha256sum -c && \
  tar -C /usr/local -xzf go.tar.gz && \
  mkdir /go && \
  chown -R "${USERID}:${USERID}" /go && \
  rm go.tar.gz
ENV GOPATH /go
ENV PATH $GOPATH/bin:/usr/local/go/bin:$PATH


USER $USERNAME
