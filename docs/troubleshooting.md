                                       # some common problems while using wazuh 

# sometimes when you go to wazuh dashboard you might see "Wazuh dashboard server is not ready"

# run the following commands
sudo systemctl restart wazuh-manager
sudo systemctl restart wazuh-indexer
sudo systemctl restart wazuh-dashboard

# after running these commands wait for some time for wazuh services to start
# to verify run 
sudo systemctl status wazuh-manager
sudo systemctl status wazuh-indexer
sudo systemctl status wazuh-dashboard

# to make sure wazuh is recieving logs run 
sudo filebeat test output

                                       # some common problems while using logstash

# is logstash does not start automatically run 
sudo systemctl restart logstash

# if logstash is still not sending logs then run following commands and make sure to use your own paths for the executable, the pipeline, and the configuration files.
sudo systemctl stop logstash
sudo -E /usr/share/logstash/bin/logstash -f /etc/logstash/conf.d/wazuh.conf --path.settings /etc/logstash/

# look for any errors 

# after running these commands run 
sudo systemctl restart logstash.service


                                       # some common problems while using mongoDB

# you will not see any errors in mongoDB since it is running in a cloud environment, but make sure to add your ip address in the network tab to allow access.
