FROM ubuntu:22.04
RUN apt-get update \
	&& apt install -y git virtualenvwrapper curl vim build-essential zlib1g-dev libffi-dev libssl-dev libbz2-dev libncurses5-dev libreadline-dev libsqlite3-dev liblzma-dev
#RUN curl https://pyenv.run | bash

RUN mkdir /files
RUN mkdir /scripts
COPY scripts/* /scripts/
RUN echo "set mouse-=a">>/root/.vimrc
RUN echo "syntax on">>/root/.vimrc
CMD ["/scripts/start"]

