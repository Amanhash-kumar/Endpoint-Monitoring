# installing wazuh-server 
 
sudo apt update 

curl -sO https://packages.wazuh.com/4.12/wazuh-install.sh && sudo bash ./wazuh-install.sh -a 

this command will download and install all wazuh components 
make note of the instructions and wazuh dashboard username and password after this command is completed
                                   
                                   
                                   
                                   
# installing logstash
                                   
                            
1. Install prerequisite packages
sudo apt update
sudo apt-get install apt-transport-https
2. Import Elastic GPG key
wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo gpg --dearmor -o /usr/share/keyrings/elastic-keyring.gpg

3. Add Elastic APT repository
echo "deb [signed-by=/usr/share/keyrings/elastic-keyring.gpg] https://artifacts.elastic.co/packages/9.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-9.x.list

5. Install Logstash
sudo apt-get update && sudo apt-get install logstash

now install logstash-output-mongodb plugin

sudo /usr/share/logstash/bin/logstash-plugin install logstash-output-mongodb

now create a /etc/logstash/templates directory 
sudo mkdir /etc/logstash/templates
sudo curl -o /etc/logstash/templates/wazuh.json https://packages.wazuh.com/integrations/elastic/4.x-8.x/dashboards/wz-es-4.x-8.x-template.json 

add logstash user to wazuh group

sudo usermod -a -G wazuh logstash
 
enabling logstash service
sudo systemctl enable logstash.service
sudo systemctl start logstash.service 

run these commands after configuring .conf file                                              


# setting up Redash
                                    
we will install redash in docker 

to install docker run the following commands
sudo apt update
sudo apt install docker.io -y
sudo systemctl enable --now docker
sudo apt install docker-compose -y

now clone redash setup 
git clone https://github.com/getredash/setup.git redash-setup
cd redash-setup

run docker setup
./setup.sh

open terminal of redash inside docker 
docker exec -it redash-server-1 bash

now add a user
python manage.py users create "example@gmail.com" "Admin User" --password "admin123" --admin

access redash on browser
http://localhost:5000




