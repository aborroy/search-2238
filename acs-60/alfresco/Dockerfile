FROM alfresco/alfresco-content-repository:6.0.1.4

# Apply patch
RUN rm /usr/local/tomcat/webapps/alfresco/WEB-INF/lib/alfresco-enterprise-repository-*.jar
COPY alfresco-enterprise-repository-*.jar /usr/local/tomcat/webapps/alfresco/WEB-INF/lib/

# Apply new Hazelcast version
RUN rm /usr/local/tomcat/webapps/alfresco/WEB-INF/lib/hazelcast-*.jar
COPY hazelcast-*.jar /usr/local/tomcat/webapps/alfresco/WEB-INF/lib/
