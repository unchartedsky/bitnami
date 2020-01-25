FROM bitnami/wordpress-nginx:latest

RUN apt-get update && apt-get install -y \
        html2text \
        vim \
        && rm -rf /var/lib/apt/lists/*

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

WORKDIR /
