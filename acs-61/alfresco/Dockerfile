FROM alfresco/alfresco-content-repository:6.1.1.3

USER root

# Apply patch
RUN rm /usr/local/tomcat/webapps/alfresco/WEB-INF/lib/alfresco-enterprise-repository-*.jar
COPY alfresco-enterprise-repository-*.jar /usr/local/tomcat/webapps/alfresco/WEB-INF/lib/
