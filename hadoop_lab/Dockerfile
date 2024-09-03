FROM ubuntu

# Actualizar el índice de paquetes y instalar paquetes necesarios
RUN apt update && apt-get install -y \
    vim \
    python3 \
    openjdk-8-jdk \
    net-tools \
    dnsutils \
    wget \
    python3-pip

# Instalar virtualenv
RUN pip3 install virtualenv

# Crear y activar un entorno virtual
RUN python3 -m venv /opt/venv
ENV VIRTUAL_ENV=/opt/venv
ENV PATH="$VIRTUAL_ENV/bin:$PATH"

# Descargar e instalar Hadoop
RUN wget https://dlcdn.apache.org/hadoop/common/hadoop-3.4.0/hadoop-3.4.0.tar.gz && \
    tar -xvzf hadoop-3.4.0.tar.gz && mv hadoop-3.4.0 /opt && cd /opt && ln -s hadoop-3.4.0 hadoop && \
    rm -r hadoop-3.4.0.tar.gz

# Crear directorios necesarios para HDFS
RUN mkdir -p /opt/hdfs/namenode && mkdir -p /opt/hdfs/datanode

# Instalar mrjob en el entorno virtual
RUN /opt/venv/bin/pip install mrjob

# Copiar configuraciones de Hadoop
COPY config/* /opt/hadoop-3.4.0/etc/hadoop/

# Configurar variables de entorno para Hadoop y Java
ENV HADOOP_HOME=/opt/hadoop-3.4.0
ENV HADOOP_MAPRED_HOME=$HADOOP_HOME
ENV HADOOP_STREAMING_JAR=$HADOOP_HOME/share/hadoop/tools/lib/hadoop-streaming-3.4.0.jar
ENV PATH=$PATH:$HADOOP_HOME
ENV PATH=$PATH:$HADOOP_HOME/bin
ENV JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
