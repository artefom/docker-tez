FROM prasanthj/docker-hadoop
MAINTAINER Prasanth Jayachandran

# download, extract and copy tez binaries
RUN curl -s https://s3-eu-west-1.amazonaws.com/seq-tez/tez-0.5.0.tar.gz | tar -xz -C /usr/local/
RUN cd /usr/local && ln -s tez-0.5.0 tez
RUN $BOOTSTRAP && $HADOOP_PREFIX/bin/hadoop dfsadmin -safemode leave && $HADOOP_PREFIX/bin/hdfs dfs -put /usr/local/tez-0.5.0 /tez

# add tez specific configs
ADD tez-site.xml $HADOOP_PREFIX/etc/hadoop/tez-site.xml
ADD mapred-site.xml $HADOOP_PREFIX/etc/hadoop/mapred-site.xml

# environment settings
RUN echo 'TEZ_JARS=/usr/local/tez/*' >> $HADOOP_PREFIX/etc/hadoop/hadoop-env.sh
RUN echo 'TEZ_LIB=/usr/local/tez/lib/*' >> $HADOOP_PREFIX/etc/hadoop/hadoop-env.sh
RUN echo 'TEZ_CONF=/usr/local/hadoop/etc/hadoop' >> $HADOOP_PREFIX/etc/hadoop/hadoop-env.sh
RUN echo 'export HADOOP_CLASSPATH=$HADOOP_CLASSPATH:$TEZ_CONF:$TEZ_JARS:$TEZ_LIB' >> $HADOOP_PREFIX/etc/hadoop/hadoop-env.sh

# execute hadoop bootstrap script
CMD ["/etc/bootstrap.sh", "-d"]
