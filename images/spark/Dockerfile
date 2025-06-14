#+-----------------------------------------+
#|           Imagem Spark 3.5.3            | 
#|                                         |
#| O que decide se é Driver ou Worker      |
#| é o comando de inicialização no compose |
#+-----------------------------------------+

FROM ubuntu:22.04

SHELL ["/bin/bash", "-c"]

RUN apt-get update --fix-missing

#+-----------------+
#| Instalando cURL |
#+-----------------+

RUN apt-get install curl -y

#+--------------------+
#| Instalando Java 11 |
#+--------------------+

RUN apt-get install openjdk-18-jdk -y                                           && \
    echo "export JAVA_HOME=/usr/lib/jvm/java-18-openjdk-amd64" >> ~/.bashrc     && \
    echo "export PATH=\$JAVA_HOME/bin:\$PATH" >> ~/.bashrc

ENV JAVA_HOME /usr/lib/jvm/java-18-openjdk-amd64
ENV PATH "${JAVA_HOME}/bin:$PATH"

 #+------------------------+
 #| Instalando Spark 3.3.2 |
 #+------------------------+

RUN curl -O https://dlcdn.apache.org/spark/spark-3.5.3/spark-3.5.3-bin-hadoop3.tgz &&\
    tar zxvf spark-3.5.3-bin-hadoop3.tgz -C /usr/local &&\
    rm spark-3.5.3-bin-hadoop3.tgz &&\
    cd /usr/local &&\
    ln -sT spark-3.5.3-bin-hadoop3 spark

RUN echo "export SPARK_HOME=/usr/local/spark" >> ~/.bashrc
RUN echo "export PATH=\$SPARK_HOME/bin:\$PATH" >> ~/.bashrc

ENV SPARK_HOME /usr/local/spark
ENV PATH "${SPARK_HOME}/bin:$PATH" 

#+----------------------------------+
#| Instalando dependências S3/MinIO |
#+----------------------------------+

RUN curl https://repo1.maven.org/maven2/org/apache/hadoop/hadoop-aws/3.3.4/hadoop-aws-3.3.4.jar \
    --output ${SPARK_HOME}/jars/hadoop-aws-3.3.4.jar

RUN curl https://repo1.maven.org/maven2/com/amazonaws/aws-java-sdk-bundle/1.12.262/aws-java-sdk-bundle-1.12.262.jar \
    --output ${SPARK_HOME}/jars/aws-java-sdk-bundle-1.12.262.jar

RUN curl https://jdbc.postgresql.org/download/postgresql-42.5.0.jar \
    --output ${SPARK_HOME}/jars/postgresql-42.5.0.jar
    
#+---------------------------------------------+
#| Instalando dependências para Apache Iceberg |
#+---------------------------------------------+

#RUN curl https://repo1.maven.org/maven2/org/apache/iceberg/iceberg-spark-runtime-3.2_2.13/1.3.0/iceberg-spark-runtime-3.2_2.13-1.3.0.jar \
#    --output ${SPARK_HOME}/jars/iceberg-spark-runtime-3.2_2.13-1.3.0.jar

USER root