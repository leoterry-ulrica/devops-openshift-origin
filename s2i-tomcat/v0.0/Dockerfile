FROM maven:3.3-jdk-8
MAINTAINER weifj

LABEL io.openshift.s2i.scripts-url=image:///usrbexec/s2i \
      io.k8s.description="Tomcat S2I Builder" \
      io.k8s.display-name="tomcat s2i builder 1.0" \
      io.openshift.expose-services="8080:http" \
      io.openshift.tags="builder,tomcat"
WORKDIR /opt

ADD ./apache-tomcat-8.5.5.tar.gz /opt

RUN chmod -R a+rw /opt && \
chmod a+rwx /opt/apache-tomcat-8.5.5/* && \
chmod +x /opt/apache-tomcat-8.5.5/bin/*.sh && \
rm -rf /opt/apache-tomcat-8.5.5/webapps/*
# 添加maven settings.xml，修改为阿里仓库
ADD ./contrib/settings.xml /$HOME/.m2/
COPY ./s2i/bin/ /usrbexec/s2i

RUN chown -R 1001:0 $HOME

USER 1001
EXPOSE 8080
ENTRYPOINT []
CMD ["usage"]
