FROM bitnami/wordpress-nginx:latest

USER root

RUN apt-get update && apt-get install -y \
        html2text \
        vim \
        && rm -rf /var/lib/apt/lists/*

# Install ImageMagick PHP extension
# See https://github.com/bitnami/bitnami-docker-php-fpm/issues/121
RUN apt-get update && \
    apt-get -y install gcc && \
    install_packages \
      fontconfig-config fonts-dejavu-core imagemagick-6-common \
      libfftw3-double3 libfontconfig1 libglib2.0-0 libgomp1 libjbig0 \
      liblcms2-2 liblqr-1-0 libltdl7 libmagickcore-6.q16-6 \
      libmagickwand-6.q16-6 libopenjp2-7 libtiff5 libx11-6 libx11-data libxau6 \
      libxcb1 libxdmcp6 libxext6 unzip \
      gcc make autoconf libc-dev pkg-config libmagickwand-dev && \
    echo "" | pecl install redis imagick && \
    echo "" >> /opt/bitnami/php/etc/php.ini && \
    echo "extension=imagick\n" > /opt/bitnami/php/etc/conf.d/imagemagick.ini  && \
    echo "[www]\nenv[MAGICK_CODER_MODULE_PATH]='$(find /usr/lib/x86_64-linux-gnu -maxdepth 1 -type d -name ImageMagick-*)/modules-Q16/coders'" > /opt/bitnami/php/etc/php-fpm.d/imagemagick.conf && \
    apt-get -y remove --auto-remove \
      gcc make autoconf libc-dev pkg-config libmagickwand-dev && \
    rm -rf /usr/include/* && \
    rm -rf /tmp/* && \
    rm -rf /var/lib/apt/lists/*

# Install NewRelic
ENV NR_INSTALL_SILENT true

WORKDIR /tmp

RUN export NEWRELIC_FILENAME=$(curl -L --silent https://download.newrelic.com/php_agent/release | html2text | grep linux.tar.gz | cut -d ' ' -f1) && \
        curl --silent -L "https://download.newrelic.com/php_agent/release/${NEWRELIC_FILENAME}" -o "${NEWRELIC_FILENAME}" && \
        tar xvfz "${NEWRELIC_FILENAME}" && \
        cd "$(basename ${NEWRELIC_FILENAME} .tar.gz)" && \
        ./newrelic-install install && \
	cd .. && \
	rm -f "${NEWRELIC_FILENAME}"

COPY newrelic.ini /opt/bitnami/php/conf/conf.d/newrelic.ini

USER 1001

WORKDIR /
