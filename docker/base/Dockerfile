FROM openjdk:8u212-b04-jdk-stretch
LABEL author="Pradeep Sekar" email="pradipraniboy@gmail.com"
LABEL version="0.1"

ENV DAEMON_RUN=true
ENV SPARK_VERSION=3.0.1
ENV HADOOP_VERSION=3.2
ENV SCALA_VERSION=2.12.10
ENV SCALA_HOME=/usr/share/scala
ENV SPARK_HOME=/spark

##  Get all certificates.
RUN apt-get update && apt-get install -y curl vim wget software-properties-common ssh net-tools ca-certificates jq


##  Install all initial lirary 
RUN cd "/tmp" && \
    wget --no-verbose "https://downloads.typesafe.com/scala/${SCALA_VERSION}/scala-${SCALA_VERSION}.tgz" && \
    tar xzf "scala-${SCALA_VERSION}.tgz" && \
    mkdir "${SCALA_HOME}" && \
    rm "/tmp/scala-${SCALA_VERSION}/bin/"*.bat && \
    mv "/tmp/scala-${SCALA_VERSION}/bin" "/tmp/scala-${SCALA_VERSION}/lib" "${SCALA_HOME}" && \
    ln -s "${SCALA_HOME}/bin/"* "/usr/bin/" && \
    rm -rf "/tmp/"*

# Add Dependencies for PySpark
RUN apt-get install -y python3 python3-pip python3-numpy python3-matplotlib python3-scipy python3-pandas python3-simpy
RUN update-alternatives --install "/usr/bin/python" "python" "$(which python3)" 1


#Scala instalation ---> Check the spark and sbt versions
RUN export PATH="/usr/local/sbt/bin:$PATH" &&  apt update && apt install ca-certificates wget tar && mkdir -p "/usr/local/sbt" && wget -qO - --no-check-certificate "https://github.com/sbt/sbt/releases/download/v1.2.8/sbt-1.2.8.tgz" | tar xz -C /usr/local/sbt --strip-components=1 && sbt sbtVersion

RUN wget --no-verbose http://apache.mirror.iphh.net/spark/spark-${SPARK_VERSION}/spark-${SPARK_VERSION}-bin-hadoop${HADOOP_VERSION}.tgz && tar -xvzf spark-${SPARK_VERSION}-bin-hadoop${HADOOP_VERSION}.tgz \
      && mv spark-${SPARK_VERSION}-bin-hadoop${HADOOP_VERSION} spark \
      && rm spark-${SPARK_VERSION}-bin-hadoop${HADOOP_VERSION}.tgz

# Fix the value of PYTHONHASHSEED
# Note: this is needed when you use Python 3.3 or greater
ENV PYTHONHASHSEED 1