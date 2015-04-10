# Introduction #
This document walks you through the steps to manually install a Google Search Appliance Connector for File Systems. The instructions mentioned here are applicable to connector version 1.0.2\_ALPHA and above

Google recommends that you use the installer for installing the connector. You may want to do it manually if,

  * You have built and installed a customized connector manager or a customized version of   the connector
  * You want to deploy the connector on an existing Tomcat installation
  * You are installing a patch release that is not packaged with an installer.

# Pre-requisites #
  * Apache Tomcat 6. You can download it from it from http://tomcat.apache.org/download-60.cgi
  * JRE1.5 or above. Refer to http://java.sun.com/javase/downloads/index.jsp for downloads

# Manual installation steps #
  1. Shut down Tomcat if it is running.
  1. Go to $CATALINA\_HOME\bin directory.
    * **For Windows** edit setclasspath.bat
```
      Add the following lines in the start of the file:
      set JRE_HOME=<JRE_HOME>
      set PATH=%PATH%;
   
      Add the following lines just before "rem Set standard command for invoking Java":
      rem Google Search Appliance Connector Logging
      set CONNECTOR_LOGGING=%CATALINA_HOME%\webapps\connector-manager\WEB-INF\lib\connector-logging.jar
      if not exist "%CONNECTOR_LOGGING%" goto noConnectorLogging
      set CLASSPATH=%CLASSPATH%;%CONNECTOR_LOGGING%
      :noConnectorLogging
```
    * **For Linux** edit setclasspath.sh
```
      Add the following lines in the start of the file:
      export JRE_HOME=<JRE_HOME>
      export PATH="$PATH":"$JRE_HOME"/bin
     
      Add the following lines just before "# OSX hack to CLASSPATH":
      # Google Search Appliance Connector Logging
      CONNECTOR_LOGGING="$CATALINA_HOME"/webapps/connector-manager/WEB-INF/lib/connector-logging.jar
      if [ -f "$CONNECTOR_LOGGING ]; then
      CLASSPATH="$CLASSPATH":"$CONNECTOR_LOGGING"
      fi

```
> > > Please make sure that all the shell scripts (with ".sh" as extension) have execute permissions.
  1. If you do not have the connector manager installed, follow these steps
    * Start a web browser and navigate to http://code.google.com/p/google-enterprise-connector-manager/downloads/list
    * Download the correct binary distribution compressed file for your platform
    * unpack the binary distribution
    * copy the connector-manager.war to $CATALINA\_HOME/webapps directory
    * Start Tomcat so that the connector manager gets deployed.
    * To confirm that the connector manager has been properly deployed under Tomcat confirm that a directory with the name connector-manager has been created under $CATALINA\_HOME/webapps directory
    * Shut down the Tomcat to start with the further steps.
  1. In a browser goto http://code.google.com/p/google-enterprise-connector-file-system/downloads/list and download the binary distribution of your choice.
  1. Unzip the downloaded file into an empty root distribution directory.
  1. Copy connector-filesystem.jar from the root distribution directory to the $CATALINA\_HOME/webapps/connector-manager/WEB-INF/lib directory.
  1. Copy the files in the Lib directory under the root distribution directory to the $CATALINA\_HOME/webapps/connector-manager/WEB-INF/lib directory.
  1. Edit the file $CATALINA\_HOME/bin/catalina.bat (catalina.sh on Linux):
```
       Change the line:
         set JAVA_OPTS=%JAVA_OPTS% -Djava.util.logging.manager=org.apache.juli.ClassLoaderLogManager -Djava.util.logging.config.file="%CATALINA_BASE%\conf\logging.properties"
       To:
         set JAVA_OPTS=%JAVA_OPTS% -Djava.util.logging.manager=java.util.logging.LogManager -Djava.util.logging.config.file="%CATALINA_BASE%\webapps\connector-manager\WEB-INF\classes\logging.properties"

       For Linux use a Unix path and $CATALINA_BASE 
```
  1. In the $CATALINA\_HOME/webapps/connector-manager/WEB-INF directory, create a directory or called classes.
  1. Copy the file logging.properties file from the Config directory in the root distribution directory to the new $CATALINA\_HOME/webapps/connector-manager/WEB-INF/classes directory.
  1. Start the Tomcat server.
  1. To confirm whether the Tomcat server has started correctly and the connector is installed, navigate to the $CATALINA\_HOME/webapps/connector-manager/WEB-INF/connectors directory, and verify that the $CATALINA\_HOME/webapps/connector-manager/WEB-INF/connectors/FileConnectorType directory exists.
  1. Check and confirm if the connector type has been detected by the Connector Manager using the following URL:


> http://localhost:8080/connector-manager/getConnectorList

