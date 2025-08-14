# configuring wazuh
                                                       
open the link displayed at the end of wazuh installation and login with username and ppassword

then on the left side search for Agents Management tab and look for 'Summary' 

then click on the "Deploy New Agent" option, then follow the instructions to deploy a new agent

for example if the endpoint is using DebAmd64 os, then go to the terminal of the endoint and use the following command to 
install the wazuh agent "wget https://packages.wazuh.com/4.x/apt/pool/main/w/wazuh-agent/wazuh-agent_4.12.0-1_amd64.deb && sudo
WAZUH_MANAGER='192.168.1.6' dpkg -i ./wazuh-agent_4.12.0-1_amd64.deb"

and start the agent using following commands 
sudo systemctl daemon-reload
sudo systemctl enable wazuh-agent
sudo systemctl start wazuh-agent


# Configuring Logstash

create a file in /etc/logstash/conf.d/ directory named wazuh-to-mongodb.conf or anything of your choice
enter the following 

Input {
  File {
    Id => "Wazuh_Alerts"
    Path => "/Var/Ossec/Logs/Alerts/Alerts.Json"
    Codec => "Json"
    Start_Position => "Beginning"
    Stat_Interval => "1 Second"
    Mode => "Tail"
    Ecs_Compatibility => "Disabled"
  }
}


Output {
    Mongodb {
      Uri => "Mongodb+Srv://User:Pass@Cluster0.4pmhlcl.Mongodb.Net/?Retrywrites=True&W=Majority&Appname=Cluster0"
      Database => "Wazuh_Logs"
      Collection => "Alerts"
    }
  }

change the uri to your mongodb uri

then restart logstash using
sudo systemctl daemon-reload
sudo systemctl restart logstash

 
# setting up mongo db   
                                   
For this project we are using mongodb Atlas 
go to "https://www.mongodb.com/products/platform/atlas-database" and signup and then login
now create a new project "Wazuh_logs"
now "build a database", choose shared, free tier
and then "create cluster"
it will take some time to create a cluster
then add a user, go to "Database Access"--> "Add a new database user"
then whitelist your ip, in network setting simply click on add this ip address, this will add your current ip address 
then make note of the cluster address, to setup logstash
go to "database" --> "connect" then make note of the url which might look like this "mongodb+srv://logstash_user:<password>@cluster0.abcde.mongodb.net/logstash_logs?retryWrites=true&w=majority"


# configuring Redash


go to http://localhost:5000 to access redash and login with your credentials
from the homepage select "connect a data source"
# in the Type selection select MongoDB
# then give name to your db, provide mongodb uri in the connection string, and useranme, password, Database name respectively
# then click save, youc can also test the connection.
 
