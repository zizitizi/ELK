# ELK
elastik stack



docker-compose up -d

docker ps


 http://localhost:5601

  http://localhost:9200




Configuring ELK for log analysis
Considering the ELK Stack is already installed, we can directly configure log analysis to capture ngnix server logs from a Linux system. You can use this process for Apache logs as well. In a Linux based system, server logs are present under the /var/log directory and nginx logs are in the /var/log/nginx directory.

Follow the steps given below to configure ngnix logs in ELK.

1. First, install filebeat:

> curl -L -O https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-7.12.0-linux-x86_64.tar.gz
 
> tar xzvf filebeat-7.12.0-linux-x86_64.tar.gz
2. Update the filebeat configuration file at location in <filebeat-directory>/config/filebeat.yml. Edit the Elasticsearch details as given below:

Config
output.elasticsearch:
hosts: [“your-elasticsearch-host:9200”]
username: “YOUR_ELASTICSEARCH_USERNAME”
password: “YOUR_ ELASTICSEARCH_PASSWORD”
Also, update the Kibana details as given below:

setup.kibana:
host: “your-kibana-host:5601”
username: “YOUR_KIBANA_USERNAME”
password: “YOUR_ KIBANA_PASSWORD”
Then save this config file.

3. Now, to add ngnix as a module to filebeat run the following command:

> ./filebeat modules enable nginx
4. Update module settings in module configs under modules.d to match your environment. If your logs are in different locations, then set the paths variable:

Configs
- module: nginx
access:
var.paths: [“/var/log/nginx/*.log”]
Save <em>configs.</em>
5. Now execute the command given below to set up filebeat to take this configuration and download all the required plugins:

> ./filebeat setup –e
6. And now we start the filebeat with the commands given below:

> sudo chown root filebeat.yml
> sudo chown root modules.d/system.yml
> sudo ./filebeat –e
Once filebeat is started, it should begin streaming events to Elasticsearch.

7. We can now view the logs on Kibana, in your browser (http://your-kibana-host:5601). When you go to Left Panel πDiscover πfilebeat-*, you should be able to view all the logs, as shown in Figure 2.

Kibana filebeat ngnix logs
Figure 2: Kibana filebeat ngnix logs
This is how you can configure server logs in the ELK Stack. You can also customise the Logstash config to further process these logs.

Benefits of ELK Stack

ELK Stack works best when logs from various applications are collected into a single ELK instance.
It provides amazing insights in a single instance, which eliminates the need to go through numerous different log data sources.
Easy to scale.
Libraries for different programming languages are available.
It provides extensions to connect with other monitoring tools.
Provides a pipeline mechanism to process logs into the format you want.
Drawbacks of ELK Stack

It can become difficult to handle these different components in a stack when we move to a complex setup.
You need to keep scaling, as your infrastructure and size of logs increases continuously.


  
