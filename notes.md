Pipeline for Jenkins for Spring Pet Clinic Project in 3 methods
---------------------------------------------------------------
* spc -**pre requisites** for jenkins pipeline
  * java version - 17
  * maven version - 3.8 or above (at this time apt will install 3.6 by defualt so we have to install maven by tar file)
  ```
   wget https://dlcdn.apache.org/maven/maven-3/3.8.7/binaries/apache-maven-3.8.7-bin.tar.gz /tmp
   sudo tar -zxvf apache-maven-3.8.7-bin.tar.gz
   sudo mv apache-maven-3.8.7/ /opt/maven/
   sudo vi /etc/environment  # add env variable as :/opt/maven/bin
   source /etc/environment
   mvn -v
  ```
* here java for jenkins to work normally we require java 11 and above
* **TASK** - create jenkins pipeline for SPC
    * freestyle
    * scripted
    * declarative 