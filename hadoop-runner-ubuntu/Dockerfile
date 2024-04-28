FROM ubuntu:latest  
  
# Install prerequisites  

RUN apt-get update && \  
    apt-get install -y sudo wget netcat-openbsd jq openjdk-8-jre-headless curl tar python3-pip && \  
    ln -s /usr/bin/python3 /usr/bin/python && \  
    apt-get clean && \  
    rm -rf /var/lib/apt/lists/*  
  
# Install pip and robotframework  
RUN pip install robotframework  --break-system-packages 
  
# Install dumb-init  
RUN wget -O /usr/local/bin/dumb-init https://github.com/Yelp/dumb-init/releases/download/v1.2.0/dumb-init_1.2.0_amd64 && \  
    chmod +x /usr/local/bin/dumb-init  
  
# Prepare keytabs directory  
RUN mkdir -p /etc/security/keytabs && chmod -R a+wr /etc/security/keytabs  
  
# Add Byteman JAR  
ADD https://repo.maven.apache.org/maven2/org/jboss/byteman/byteman/4.0.4/byteman-4.0.4.jar /opt/byteman.jar  
RUN chmod o+r /opt/byteman.jar  
  
# Install async-profiler  
RUN mkdir -p /opt/profiler && \  
    cd /opt/profiler && \  
    curl -L https://github.com/jvm-profiling-tools/async-profiler/releases/download/v1.5/async-profiler-1.5-linux-x64.tar.gz | tar xvz  
  
# Set JAVA_HOME  
ENV JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64/  
ENV PATH=$PATH:/opt/hadoop/bin  
  
# Add hadoop user  
RUN groupadd --gid 1100 hadoop && \  
    useradd --uid 1100 --gid 1100 --home /opt/hadoop hadoop && \  
    echo "hadoop ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers && \  
    chown hadoop:hadoop /opt  
  
# Copy scripts and krb5.conf  
ADD scripts /opt/  
ADD scripts/krb5.conf /etc/  

RUN chmod +x /opt/starter.sh    
RUN chmod +x /opt/envtoconf.py
RUN chmod +x /opt/transformation.py  

# Install Kerberos client  
RUN apt-get update && \  
    apt-get install -y krb5-user && \  
    apt-get clean && \  
    rm -rf /var/lib/apt/lists/*  
  
# Prepare Hadoop directories  
RUN mkdir -p /etc/hadoop && mkdir -p /var/log/hadoop && chmod 1777 /etc/hadoop && chmod 1777 /var/log/hadoop  
  
# Set environment variables  
ENV HADOOP_LOG_DIR=/var/log/hadoop  
ENV HADOOP_CONF_DIR=/etc/hadoop  
  
# Set the working directory  
WORKDIR /opt/hadoop  
  
# Create a data volume  
VOLUME /data  
  
# Use 'hadoop' user  
USER hadoop  
# RUN chmod -R a+rwx /opt/hadoop  


# Set entrypoint  
ENTRYPOINT ["/usr/local/bin/dumb-init", "--", "/opt/starter.sh"]  
