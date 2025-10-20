FROM centos:centos6.8

RUN rm -rf /etc/yum.repos.d/*
COPY files/vault.repo /etc/yum.repos.d/centos-vault.repo
RUN rpm --import /etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6
#COPY files/rust-1.59.0-x86_64-unknown-linux-gnu.tar.gz /

#We install rust because Paramiko requires it for a depedency. Probably other things as well.
RUN yum groupinstall "Development Tools" -y \
	&& yum install -y zlib-devel libffi-devel openssl-devel git curl bzip2-devel ncurses-devel readline-devel sqlite-devel xz-devel mysql-devel.x86_64 mysql.x86_64 httpd-devel.x86_64 httpd.x86_64 mysql-libs.x86_64 wget \
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
COPY scripts/* /scripts/
CMD ["/scripts/start"]
