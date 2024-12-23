# hadoop-runner-ubuntu
updated from apache hadoop-runner

Base image on ubuntu (instead of Centos) ready for Apache Hadoop installation  

The built image tag on Docker Hub is asami65/hadoop-runner-ubuntu:latest


# Step 0: Prereqs
Make sure you have the following installed  
* `$ sudo apt install python3-pip wget git curl docker-compose`
* `$ curl -fsSL https://test.docker.com -o test-docker.sh`
* `$ sudo sh test-docker.sh`
* `$ sudo usermod -aG docker <username>`
* `$ git clone https://github.com/ahmedsami76/hadoop3.git`
* Install vscode

# Step 1: Build the `hadoop-runner-ubuntu` image
`$ docker build -t hadoop-runner-ubuntu ~/hadoop3/hadoop-runner-ubuntu` # you might want to change the path as needed

# Step 2: Build the `hadoop` image
`$ docker build -t hadoop-runner-ubuntu ~/hadoop3/hadoop` # you might want to change the path as needed

# Step 3: Run the `docker-hoster` container
`$ docker run -d -v /var/run/docker.sock:/tmp/docker.sock -v /etc/hosts:/tmp/hosts asami76/docker-hoster`

# Step 3: Run the docker-compose to build the environment
`$ docker-compose -f ~/hadoop3/docker-compose/docker-compose.yaml up -d`

# Step 4: Test the deployment
* Browse to http://http://localhost:9870 and make sure you can see the HDFS page
* From the "Utilities" menu choose "Browse the file system" and make sure you can see the list of directories which are empty for the moment
* Attach to the shell of the namenode
* `$ hdfs dfs -mkdir /testdir`
* `$ hdfs dfs -copyFromLocal /etc/hosts /testdir/hosts`
* From the browser refresh the page and make sure you can now see a dir called "testdir" and it has a "hosts" file inside it


----
```
# Example (may vary depending on your distribution)
wget https://archive.apache.org/dist/hadoop/common/hadoop-3.3.6/hadoop-3.3.6.tar.gz # Replace with desired version
tar -xzf hadoop-3.3.6.tar.gz
cd hadoop-3.3.6
export HADOOP_HOME=$PWD
export PATH=$PATH:$HADOOP_HOME/bin
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:$HADOOP_HOME/lib/native
sudo apt-get install -y default-jdk
```
