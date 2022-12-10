FROM ubuntu:20.04 AS sam

RUN set -x \
  && : "Install SAM CLI" \
  && apt-get update -qq \
  && apt-get install -qq -y --no-install-recommends \
  apt-utils \
  ca-certificates \
  unzip \
  wget \
  && samVersion=v1.66.0 \
  && samArchive=aws-sam-cli-linux-x86_64.zip \
  && samExpandDir=sam-installation \
  && wget --quiet https://github.com/aws/aws-sam-cli/releases/download/${samVersion}/${samArchive} \
  && unzip -q $samArchive -d $samExpandDir \
  && ${samExpandDir}/install \
  && rm -r $samArchive $samExpandDir \
  && apt-get clean \
  && rm -rf /var/lib/apt/lists/*

FROM ubuntu:20.04 AS base

ARG USERNAME=vscode
ARG USER_UID=1000
ARG USER_GID=$USER_UID

ENV DEBIAN_FRONTEND=noninteractive

RUN set -x \
  && : "Create the user" \
  && groupadd --gid $USER_GID $USERNAME \
  && useradd --uid $USER_UID --gid $USER_GID -m $USERNAME

COPY --from=sam /usr/local/aws-sam-cli /usr/local/aws-sam-cli
RUN set -x \
  && : "Install SAM" \
  && ln -s /usr/local/aws-sam-cli/current/bin/sam /usr/local/bin/sam

ENV DEBIAN_FRONTEND=dialog

USER $USERNAME
CMD ["sam", "-h"]

FROM base AS production

FROM base AS development

ARG DOCKER_GID=1001

USER root

RUN set -x \
  # && DOCKER_VERSION_STRING=5:20.10.13~3-0~ubuntu-jammy \
  && : "Set up the repository" \
  && apt-get update -qq \
  && apt-get install -qq -y --no-install-recommends\
  ca-certificates \
  curl \
  gnupg \
  lsb-release \
  && mkdir -p /etc/apt/keyrings \
  && curl -fsSL https://download.docker.com/linux/ubuntu/gpg | gpg --dearmor -o /etc/apt/keyrings/docker.gpg \
  && echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" \
  | tee /etc/apt/sources.list.d/docker.list > /dev/null \
  && : "Install docker engine" \
  && apt-get update -qq \
  && apt-get install -qq -y --no-install-recommends \
  docker-ce \
  docker-ce-cli \
  containerd.io \
  docker-compose-plugin \
  && : "Set docker group" \
  && groupadd -g $DOCKER_GID dood \
  && usermod -aG dood $USERNAME \
  && : "Clean" \
  && apt-get clean -qq \
  && rm -rf /var/lib/apt/lists/*

USER $USERNAME
