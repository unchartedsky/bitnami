FROM bitnami/wordpress-nginx:latest

# install NewRelic
ENV NEWRELIC_AGENT_VERSION php5-9.5.0.252
ENV NR_INSTALL_SILENT true

WORKDIR /tmp

RUN curl --silent -L https://download.newrelic.com/php_agent/release/newrelic-${NEWRELIC_AGENT_VERSION}-linux.tar.gz -o newrelic-${NEWRELIC_AGENT_VERSION}-linux.tar.gz && \
        tar xvfz newrelic-${NEWRELIC_AGENT_VERSION}-linux.tar.gz && \
        cd "newrelic-${NEWRELIC_AGENT_VERSION}-linux" && \
        ./newrelic-install install && \
	cd .. && \
	rm -f newrelic-${NEWRELIC_AGENT_VERSION}-linux.tar.gz

COPY newrelic.ini /opt/bitnami/php/conf/conf.d/newrelic.ini

WORKDIR /
