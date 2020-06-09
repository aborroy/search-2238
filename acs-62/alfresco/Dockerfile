FROM alfresco/alfresco-content-repository:6.2.1

USER root

# Apply patch
RUN rm /usr/local/tomcat/webapps/alfresco/WEB-INF/lib/alfresco-enterprise-repository-*.jar
COPY alfresco-enterprise-repository-*.jar /usr/local/tomcat/webapps/alfresco/WEB-INF/lib/
