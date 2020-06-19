# ELK-Elastalert
Filebeat config with some Elastalert rules configuration


# Step 1. Install Docker and run ELK container 
1. docker run --restart=always -e TZ=Asia/Ho_Chi_Minh -p 5601:5601 -p 9200:9200 -p 5044:5044 -d  --name elk sebp/elk:771
# Step 2. Install and configure filebeat to send log to logstash.
1. curl -L -O 30-output.confhttps://artifacts.elastic.co/downloads/beats/filebeat/filebeat-7.7.1-x86_64.rpm
2. sudo rpm -vi filebeat-7.7.1-x86_64.rpm
3. Edit file /etc/filebeat/filebeat.yml  to point to logstash ( uploaded filebeat.yml in the git repo)
# Step 3. Configure logstash on ELK container to accept request from filebeat and configure grok pattern for apache log file.
1. Exec to docker ELK container
2. Edit file /etc/logstash/conf.d/02-beats-input.conf for grok pattern to parse apache log file ( uploaded 02-beats-input.conf in the git repo)
3. Edit file /etc/logstash/conf.d/30-output.conf for logstash to output to elasticsearch ( uploaded 30-output.yml in the git repo)
4. Restart container to apply config
5. Start filebeat from STEP 2.

# Step 4. Install Elastalert

1. Install python3 and pip3
2. pip3 install elastalert
3. Create slack channel, create webhook and add to the channel. 
4. Edit config.yaml for elastalert to point to ES node. ( uploaded config.yaml in the git repo)
5. Create rules file in rules_folder with slack webhook url ( uploaded rules_folder in the git repo)
6. Change directory to elastalert ( with config file and rules_folder)
7. Start elastalert "python3 -m elastalert.elastalert --verbose"

# Step 5. Get the example log file and in order for filebeat to send log to logstash.
1. wget  https://devopsplayground.blob.core.windows.net/playground/sample-access-log-20200515.log
2. split sample-access-log-20200515.log split.log to divide to smaller log file , default 1000 lines for each small log file. To split log data 
3. Put log file to filebeat gather folder, filebeat will send log file to logstash.
4. Check slack for alerts
# Step 6. Draw graph in kibana
exported kibana dashboard file export.ndjson ( uploaded config.yaml in the git repo)




