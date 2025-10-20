FROM centos:centos7.9.2009

#Setup  yum repos for Centos Vault since we're dealing with long past EOL stuff.
RUN rm -rf /etc/yum.repos.d
COPY files/CentOS-Vault.repo /etc/yum.repos.d/

#COPY files/rust-1.59.0-x86_64-unknown-linux-gnu.tar.gz /
#COPY files/virtualenv-15.2.0.tar.gz /



#RUN chmod 444 /etc/yumd.repos.d/CentOS-Vault.repo 
#RUN chmod 555 /etc/yumd.repos.d/CentOS-Vault.repo

#We install rust because Paramiko requires it for a depedency. Probably other things as well.
RUN yum groupinstall "Development Tools" -y \
	&& yum install -y gcc make zlib-devel bzip2 bzip2-devel readline-devel sqlite sqlite-devel openssl-devel tk-devel libffi-devel xz-devel git curl ncurses-devel wget \
	&& wget https://files.pythonhosted.org/packages/b1/72/2d70c5a1de409ceb3a27ff2ec007ecdd5cc52239e7c74990e32af57affe9/virtualenv-15.2.0.tar.gz \
	&& wget https://static.rust-lang.org/dist/rust-1.59.0-x86_64-unknown-linux-gnu.tar.gz \
	&& tar zxvf rust-1.59.0-x86_64-unknown-linux-gnu.tar.gz \
    	&& rust-1.59.0-x86_64-unknown-linux-gnu/install.sh \
    	&& tar zxvf virtualenv-15.2.0.tar.gz \
    	&& cd virtualenv-15.2.0 \
	&& python setup.py install \
    	&& mkdir /files \
	&& mkdir /scripts \
	&& echo "set mouse-=a">>/root/.vimrc \
	&& echo "syntax on">>/root/.vimrc

#The virtualenv package is a manual download, because the Python that ships with Centos 6.8 can't handle SNI.
COPY scripts/* /scripts/
CMD ["/scripts/start"]
