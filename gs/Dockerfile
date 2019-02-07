FROM ubuntu:14.04
RUN apt-get update && apt-get install -y software-properties-common && \
    add-apt-repository -y ppa:openjdk-r/ppa && \
    apt-get update && \
    apt-get -y install openjdk-8-jdk mysql-client wget p7zip-full && \
    wget https://sourceforge.net/projects/globalsight/files/GlobalSight_Software/GlobalSight_8.7.3/GlobalSight_Software_Package_Linux_8.7.3.zip/download && \
    mv download GlobalSight_Software_Package_Linux_8.7.3.zip && \
    useradd -ms /bin/bash jboss && \
    7z e GlobalSight_Software_Package_Linux_8.7.3.zip -o/app GlobalSight/*.* && \
    chmod -R 777 /app && \
    rm GlobalSight_Software_Package_Linux_8.7.3.zip && \
    apt-get remove -y software-properties-common wget p7zip-full && \
    apt-get autoremove -y && \
    export JAVA_HOME=$(dirname $(dirname $(update-alternatives --list javac))) && \
    export PATH=$JAVA_HOME/bin:$PATH
USER jboss
WORKDIR /home/jboss
RUN jar xvf /app/GlobalSight.zip && \
    chmod u+x /home/jboss/jboss/server/bin/standalone.sh && \
    chmod u+x /home/jboss/jboss/server/bin/jboss-cli.sh && \
    chown -R jboss:jboss /home/jboss
ADD d.properties /home/jboss/
USER root
WORKDIR /home/jboss
CMD cd install && (echo n; sleep 10; echo 6; sleep 10; echo 4; sleep 5; echo t; sleep 5; echo i; sleep 5;echo y) | java -classpath .:lib/commons-codec-1.3.jar:installer.jar:lib/ant.jar:lib/log4j.jar Install -properties /home/jboss/d.properties && \
    /home/jboss/jboss/server/bin/standalone.sh
