#Build base image
FROM ubuntu:20.04

#Set Time Zone && postgresql:
RUN apt-get update && DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends \
        tzdata \
    && rm -rf /var/lib/apt/lists/* \
    && apt-get update

#Install general packages and dependencies:
RUN apt-get update && apt-get -y install openjdk-11-jdk wget nano maven ant git curl supervisor gettext-base cron && \
    update-alternatives --config java && \
    apt-get clean && rm -rf /var/lib/apt/lists/*

#Create defualt working directory:
RUN mkdir -p /usr/local/solr8

#Set defualt working directory:
WORKDIR /usr/local/solr8

#Add Source Code To working directory:
ADD . /usr/local/solr8

#Setting supervisor service and creating directories for copy supervisor configurations:
RUN mkdir -p /var/log/supervisor && mkdir -p /etc/supervisor/conf.d && \
    cp pre_config_files/supervisor.conf /etc/supervisor.conf

#Create defualt working directories && install solr:
RUN mkdir -p /opt/solr \
     #set solr user:
     && groupadd solr && useradd -s /bin/false -g solr -d /opt/solr solr \
     && apt-get update \
     && tar xvzf pre_config_files/solr-8.5.2.tgz \
     && cp -r solr-8.5.2/* /opt/solr \
     && chown -Rv solr: /opt/solr \
     && rm -r solr-8.5.2 && apt-get clean && rm -rf /var/lib/apt/lists/*

#Add Deployment Files To working directory:
COPY --chown=solr:solr dspace_solr_core/authority /opt/solr/server/solr/configsets/authority
COPY --chown=solr:solr dspace_solr_core/oai /opt/solr/server/solr/configsets/oai
COPY --chown=solr:solr dspace_solr_core/search /opt/solr/server/solr/configsets/search
COPY --chown=solr:solr dspace_solr_core/statistics /opt/solr/server/solr/configsets/statistics


#Expose Solr Ports:
EXPOSE 8983

#RUN Solr from supervisord:
CMD ["supervisord", "-c", "/etc/supervisor.conf"]

