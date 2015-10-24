FROM centos:7
MAINTAINER Jason Ritenour <jason.ritenour@gmail.com>

RUN yum clean all
RUN yum install -y java-1.7.0-openjdk openssh-server supervisor 
USER root
RUN yum install -y which tar sudo openssh-clients rsync
ADD hadoop-2.5.2.tar.gz /root/hadoop-2.5.2.tar.gz

RUN mv /root/hadoop-2.5.2.tar.gz /root/hadoop-2.5.2
RUN mkdir /root/hadoop
RUN mv /root/hadoop-2.5.2/hadoop-2.5.2/* /root/hadoop
RUN ls /root/hadoop
RUN ssh-keygen -q -N "" -t dsa -f /etc/ssh/ssh_host_dsa_key
RUN ssh-keygen -q -N "" -t rsa -f /etc/ssh/ssh_host_rsa_key
RUN ssh-keygen -q -N "" -t rsa -f /root/.ssh/id_rsa
RUN cp /root/.ssh/id_rsa.pub /root/.ssh/authorized_keys
ENV JAVA_HOME=/usr/lib/jvm/jre
ENV HADOOP_PREFIX=/root/hadoop
ENV HADOOP_HOME=$HADOOP_PREFIX
ENV HADOOP_INSTALL=$HADOOP_HOME
ENV HADOOP_MAPRED_HOME=$HADOOP_HOME
ENV HADOOP_COMMON_HOME=$HADOOP_HOME
ENV HADOOP_HDFS_HOME=$HADOOP_HOME
ENV HADOOP_YARN_HOME=$HADOOP_HOME
ENV HADOOP_COMMON_LIB_NATIVE_DIR=$HADOOP_HOME/lib/native
ENV PATH=$PATH:$HADOOP_HOME/sbin:$HADOOP_HOME/bin
ENV JAVA_LIBRARY_PATH=$HADOOP_HOME/lib/native:$JAVA_LIBRARY_PATH

RUN source $HOME/.bashrc

ADD core-site.xml $HADOOP_PREFIX/etc/hadoop/core-site.xml
ADD hdfs-site.xml $HADOOP_PREFIX/etc/hadoop/hdfs-site.xml

ADD mapred-site.xml $HADOOP_PREFIX/etc/hadoop/mapred-site.xml
ADD yarn-site.xml $HADOOP_PREFIX/etc/hadoop/yarn-site.xml

RUN $HADOOP_PREFIX/bin/hdfs namenode -format


# workingaround docker.io build error
RUN ls -la /root/hadoop/etc/hadoop/*-env.sh
RUN chmod +x /root/hadoop/etc/hadoop/*-env.sh
RUN ls -la /root/hadoop/etc/hadoop/*-env.sh

RUN /root/hadoop/sbin/start-dfs.sh
CMD ["/root/hadoop/sbin/start-yarn.sh"]

# Hdfs ports
EXPOSE 50010 50020 50070 50075 50090
# Mapred ports
EXPOSE 19888
#Yarn ports
EXPOSE 8030 8031 8032 8033 8040 8042 8088
#Other ports
EXPOSE 49707 2122   
