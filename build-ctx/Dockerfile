FROM scratch

ARG CF_DEBIAN_RELEASE
ARG CF_DEBIAN_VERSION
ARG CF_CPUARCH_DEB_DIST
ENV CF_CPUARCH_DEB_DIST=${CF_CPUARCH_DEB_DIST}
ARG CF_CPUARCH_DEB_ROOTFS

# copy Debian root-fs
ADD cache/debian_${CF_DEBIAN_RELEASE}/rootfs-debian_${CF_DEBIAN_RELEASE}_${CF_DEBIAN_VERSION}-${CF_CPUARCH_DEB_ROOTFS}.tar.xz /

# set environment variables
ENV DEBIAN_FRONTEND="noninteractive"
ENV \
	HOME="/root" \
	LANGUAGE="en_US.UTF-8" \
	LANG="en_US.UTF-8" \
	TERM="xterm"

# ------------------------------------------------------------------------------------------
# taken from:
#   https://github.com/docker-library/repo-info/blob/master/repos/debian/remote/stretch.md
#
CMD ["/bin/bash"]
# ------------------------------------------------------------------------------------------

ENV LC_ALL="en_US.UTF-8"

RUN \
	apt-get update \
	&& apt-get upgrade -y \
	&& apt-get dist-upgrade -y \
	&& apt-get install -y --no-install-recommends \
			apt-utils \
			locales \
			curl \
			tzdata \
			nano \
			less \
			wget \
			ca-certificates \
	|| exit 1; \
	#
	echo "**** generate locale ****"; \
	sed -i'' \
			-e 's/# de_DE.UTF-8 UTF-8/de_DE.UTF-8 UTF-8/' \
			-e 's/# de_DE ISO-8859-1/de_DE ISO-8859-1/' \
			-e 's/# de_DE@euro ISO-8859-15/de_DE@euro ISO-8859-15/' \
			-e 's/# en_US.UTF-8 UTF-8/en_US.UTF-8 UTF-8/' \
			-e 's/# en_US ISO-8859-1/en_US ISO-8859-1/' \
			-e 's/# en_US@euro ISO-8859-15/en_US@euro ISO-8859-15/' \
			/etc/locale.gen \
	&& dpkg-reconfigure locales \
	|| exit 1; \
	#
	apt-get clean \
	&& rm -rf \
			/tmp/* \
			/var/lib/apt/lists/* \
			/var/tmp/*

# copy bashrc
COPY --chown=0:0 files/bash/dot_bashrc /root/.bashrc

#
ENV DEBIAN_FRONTEND=dialog

WORKDIR /root/
