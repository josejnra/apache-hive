# hive requires java 8
# https://github.com/apache/hive?tab=readme-ov-file#java
FROM openjdk:8

# --------------------------------------------
# HADOOP CONFIG
# --------------------------------------------
RUN apt-get update && \
    apt-get upgrade -y && \
    apt-get install -y software-properties-common \
        ssh \
        pdsh \
        openssh-server \
        rsync \
        net-tools \
        vim \
        python3-pip

# create ssh
RUN ssh-keygen -t rsa -P '' -f ~/.ssh/id_rsa && \
    cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys && \
    chmod 0600 ~/.ssh/authorized_keys
    
# --------------------------------------------

WORKDIR /opt

ENV HADOOP_VERSION 3.4.0
ENV HIVE_VERSION 3.1.3

ENV HADOOP_HOME=/opt/hadoop-${HADOOP_VERSION}
ENV HIVE_HOME=/opt/hive-${HIVE_VERSION}

# --------------------------------------------


# --------------------------------------------
# HADOOP INSTALLATION
# --------------------------------------------

# Include additional jars for HADOOP
ENV HADOOP_CLASSPATH=${HADOOP_HOME}/share/hadoop/tools/lib/aws-java-sdk-bundle-1.11.271.jar:${HADOOP_HOME}/share/hadoop/tools/lib/hadoop-aws-${HADOOP_VERSION}.jar

RUN wget https://downloads.apache.org/hadoop/common/hadoop-${HADOOP_VERSION}/hadoop-${HADOOP_VERSION}.tar.gz && \
    tar zxf hadoop-${HADOOP_VERSION}.tar.gz && \
    rm -r hadoop-${HADOOP_VERSION}.tar.gz


# --------------------------------------------
# HIVE INSTALLATION
# --------------------------------------------

RUN curl -L https://dlcdn.apache.org/hive/hive-${HIVE_VERSION}/apache-hive-${HIVE_VERSION}-bin.tar.gz | tar zxf - && \
    mv /opt/apache-hive-${HIVE_VERSION}-bin ${HIVE_HOME}

# adds postgres driver for hive metastore
RUN curl -o ${HIVE_HOME}/lib/postgresql-42.7.3.jar -L https://jdbc.postgresql.org/download/postgresql-42.7.3.jar
# --------------------------------------------


# --------------------------------------------
# ENV VARS
# --------------------------------------------

ENV PATH=$PATH:${HADOOP_HOME}/bin:${HIVE_HOME}/bin

# Hadoop env
ENV HDFS_NAMENODE_USER root
ENV HDFS_DATANODE_USER root
ENV HDFS_SECONDARYNAMENODE_USER root
ENV YARN_RESOURCEMANAGER_USER root
ENV YARN_NODEMANAGER_USER root
ENV YARN_CONF_DIR ${HADOOP_HOME}/etc/hadoop
ENV HADOOP_HOME ${HADOOP_HOME}
ENV HADOOP_LOG_DIR $HADOOP_HOME/logs
ENV HADOOP_CONF_DIR ${HADOOP_HOME}/etc/hadoop
ENV LD_LIBRARY_PATH ${HADOOP_HOME}/lib/native:$LD_LIBRARY_PATH

# USER hive
WORKDIR $HIVE_HOME

# expose several ports
EXPOSE 9871 9870 9869 9868 9867 9866 9865 9864 9820 9000 9083 8088

ENTRYPOINT ["/bin/bash"]
