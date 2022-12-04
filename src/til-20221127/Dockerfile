FROM ubuntu:20.04 AS sam

RUN set -x \
	&& : "Install SAM CLI" \
	&& apt-get update -qq \
	&& apt-get install -qq -y --no-install-recommends \
	unzip \
	wget \
	&& samVersion=v1.66.0 \
	&& samArchive=aws-sam-cli-linux-x86_64.zip \
	&& samExpandDir=sam-installation \
	&& wget --quiet --no-check-certificate https://github.com/aws/aws-sam-cli/releases/download/${samVersion}/${samArchive} \
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
CMD ["sam"]
