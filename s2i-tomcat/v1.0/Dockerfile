# This image provides a base for building and running WildFly applications.
# It builds using maven and runs the resulting artifacts on WildFly 11.0.0 Final

FROM centos/s2i-base-centos7

MAINTAINER weifj <weifj@dist.com.cn>

EXPOSE 8080

ENV TOMCAT_MAJOR_VERSION=8 \
    TOMCAT_MINOR_VERSION=8.0.48 \
    MAVEN_VERSION=3.5.2

LABEL io.k8s.description="Platform for building and running JEE applications on tomcat 8.0.48 and jdk1.8." " \
      io.k8s.display-name="tomcat 8.0.48" \
      io.openshift.expose-services="8080:http" \
      io.openshift.tags="builder,tomcat,tomcat8" \
      io.openshift.s2i.destination="/opt/s2i/destination" \
      com.redhat.deployments-dir="/tomcat/webapps"

# Install maven, tomcat 
RUN INSTALL_PKGS="tar unzip bc which lsof java-1.8.0-openjdk java-1.8.0-openjdk-devel" && \
    yum install -y --enablerepo=centosplus $INSTALL_PKGS && \
    rpm -V $INSTALL_PKGS && \
    yum clean all -y && \
    (curl -v https://www.apache.org/dist/maven/maven-3/$MAVEN_VERSION/binaries/apache-maven-$MAVEN_VERSION-bin.tar.gz | \
    tar -zx -C /usr/local) && \
    ln -sf /usr/local/apache-maven-$MAVEN_VERSION/bin/mvn /usr/local/bin/mvn && \
    mkdir -p $HOME/.m2 && \
    mkdir -p /tomcat && \
    (curl -v http://mirrors.shuosc.org/apache/tomcat/tomcat-$TOMCAT_L_VERSION/v$TOMCAT_M_VERSION/bin/apache-tomcat-$TOMCAT_M_VERSION.tar.gz | tar -zx --strip-components=1 -C /tomcat) && \
	rm -rf /tomcat/webapps/*

# Add s2i maven customizations
ADD ./contrib/settings.xml $HOME/.m2/

# Copy the S2I scripts from the specific language image to $STI_SCRIPTS_PATH
COPY ./s2i/bin/ $STI_SCRIPTS_PATH

RUN chown -R 1001:0 /tomcat && chown -R 1001:0 $HOME && \
    chmod -R ug+rw /tomcat && \
	chmod +x $STI_SCRIPTS_PATH/assemble && \
	chmod +x $STI_SCRIPTS_PATH/run

USER 1001

CMD $STI_SCRIPTS_PATH/usage
