FROM java:8

MAINTAINER Pooja Saxena pooja.saxena@ginkgo-analytics.com

# init ENV
ENV BISERVER_VERSION 8.0
ENV BISERVER_TAG 8.0.0.0-28
ENV PENTAHO_HOME /opt/pentaho

# apply JAVA_HOME
RUN . /etc/environment
ENV PENTAHO_JAVA_HOME $JAVA_HOME
ENV PENTAHO_JAVA_HOME /usr/lib/jvm/java-1.8.0-openjdk-amd64
ENV JAVA_HOME /usr/lib/jvm/jabva-1.8.0-openjdk-amd64


# install dependencies
RUN apt-get update; apt-get install zip -y; \
    apt-get install wget unzip git -y; \
    apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*;

RUN mkdir ${PENTAHO_HOME}

# RUN /usr/bin/wget --progress=dot:giga http://downloads/sourceforge.net/project/pentaho/Business%20Intelligence%20Server/${BISERVER_VERSION}/biserver-ce-${BISERVER_TAG}.zip -o /tmp/biserver-ce-${BISERVER_TAG}.zip; \

# download pentaho BI server
RUN /usr/bin/wget --progress=dot:giga https://sourceforge.net/projects/pentaho/files/Pentaho%208.0/server/pentaho-server-ce-8.0.0.0-28.zip  -O /tmp/pentaho-server-ce-${BISERVER_TAG}.zip
RUN /usr/bin/unzip -q /tmp/pentaho-server-ce-${BISERVER_TAG}.zip -d $PENTAHO_HOME
RUN  rm -f /tmp/pentaho-server-ce-${BISERVER_TAG}.zip $PENTAHO_HOME/biserver-ce/promptuser.sh
RUN  sed -i -e 's/\(exec ".*"\) start/\1 run/' $PENTAHO_HOME/pentaho-server/tomcat/bin/startup.sh
RUN  chmod +x $PENTAHO_HOME/pentaho-server/start-pentaho.sh
RUN  useradd -s /bin/bash -d ${PENTAHO_HOME} pentaho; chown -R pentaho:pentaho ${PENTAHO_HOME};

# always non-root user
USER pentaho
WORKDIR /opt/pentaho

EXPOSE 8080
CMD ["sh","/opt/pentaho/pentaho-server/start-pentaho.sh"]