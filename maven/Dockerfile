FROM openshift/jenkins-slave-base-centos7:v3.11

MAINTAINER Viktor Voronin <viktor_voronin@epam.com>

ENV MAVEN_VERSION=3.6.3 \
    BASH_ENV=/usr/local/bin/scl_enable \
    ENV=/usr/local/bin/scl_enable \
    PROMPT_COMMAND=". /usr/local/bin/scl_enable"
    
ENV PATH=$PATH:/opt/apache-maven-${MAVEN_VERSION}/bin

# Install Maven
RUN INSTALL_PKGS="java-11-openjdk-devel.x86_64" && \
    curl https://raw.githubusercontent.com/cloudrouter/centos-repo/master/CentOS-Base.repo -o /etc/yum.repos.d/CentOS-Base.repo && \
    curl http://mirror.centos.org/centos-7/7/os/x86_64/RPM-GPG-KEY-CentOS-7 -o /etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7 && \
    yum $DISABLES install -y $INSTALL_PKGS && \
    rpm -V java-11-openjdk-devel.x86_64 && \
    yum clean all -y 

RUN curl -L --output /tmp/apache-maven-bin.zip  https://www-eu.apache.org/dist/maven/maven-3/${MAVEN_VERSION}/binaries/apache-maven-${MAVEN_VERSION}-bin.zip && \
    unzip -q /tmp/apache-maven-bin.zip -d /opt && \
    ln -s /opt/apache-maven-${MAVEN_VERSION} /opt/maven && \
    rm /tmp/apache-maven-bin.zip && \
    mkdir -p $HOME/.m2
    
RUN yum remove java-1.8.0-openjdk-headless -y
                    
# When bash is started non-interactively, to run a shell script, for example it
# looks for this variable and source the content of this file. This will enable
# the SCL for all scripts without need to do 'scl enable'.
ADD contrib/bin/scl_enable /usr/local/bin/scl_enable
ADD contrib/bin/configure-agent /usr/local/bin/configure-agent
ADD ./contrib/settings.xml $HOME/.m2/

RUN chown -R 1001:0 $HOME && \
    chmod -R g+rw $HOME

USER 1001
