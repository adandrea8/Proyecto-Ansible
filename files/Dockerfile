FROM tomcat:9.0

RUN mkdir /opt/config

COPY app.properties /opt/config

ADD todo.war /usr/local/tomcat/webapps

EXPOSE 8080

CMD ["catalina.sh", "run"]

