# Instructions to integrate honeypot logs into the ELK Stack

## ELK Prerequisites

    sudo apt -y install default-jre default-jdk

    curl -fsSL https://artifacts.elastic.co/GPG-KEY-elasticsearch | apt-key add -

    echo "deb https://artifacts.elastic.co/packages/7.x/apt stable main" > /etc/apt/sources.list.d/elastic-7.x.list

    sudo apt update

## Installing ELK Stack for Honeypot Logs

- **ElasticSearch Installation**

        sudo su

        apt install elasticsearch -y

        vi /etc/elasticsearch/elasticsearch.yml
        - discovery.type: single-node
        - network.host: <YOUR IP>
        - http.port: 9200

  ![alt text](image.png)

  This Heap size depend on your machine

        vi /etc/elasticsearch/jvm.options
            -Xms512m
            -Xmx512m

  ![alt text](image-1.png)

- **Logstash Installation**

        apt install logstash

        cd /home/$USER/ELK

        mkdir -p /opt/logstash/vendor/geoip/

        cp GeoLite2-City.mmdb /opt/logstash/vendor/geoip

        cd cowrie/

        cp logstash-cowrie.conf /etc/logstash/conf.d/

        cd ../dino/

        cp logstash-dionaea.conf /etc/logstash/conf.d/

  ![alt text](image-2.png)

- **Kibana Installation**

        apt install kibana

        sudo mkdir /var/log/kibana

        sudo chown kibana:kibana /var/log/kibana

        vi /etc/kibana/kibana.yml
            - server.port: 5601
            - server.host: <YOUR IP>
            - elasticsearch.hosts: ["http://<YOUR IP>:9200"]
            - logging.dest: /var/log/kibana/kibana.log

  ![alt text](image-3.png)
  ![alt text](image-4.png)

- **Filebeat Installation**

        apt install filebeat

        cd /home/$USER/ELK

        cp filebeat.yml /etc/filebeat/

  ![alt text](image-5.png)

### Start the service

        systemctl enable elasticsearch logstash kibana filebeat

        systemctl start elasticsearch logstash kibana filebeat

Test elasticsearch and kibana connection

        curl -X GET "http://<YOUR IP>:9200"

![alt text](image-6.png)

Open your browser and type: `<YOUR IP>:5601`

![alt text](image-7.png)

## Installing Honeypot

- **Cowrie**

          sudo chmod -R 777 /home/$USER/ELK

          cd cowrie/

          docker compose up -d

  Check logs from cowrie

          docker logs <container_id>

  ![alt text](image-8.png)

- **dionaea**

        cd dino

        docker compose up -d

        docker exec -it <container-id> bash

        apt update

        apt install -y git nano sqlite3 cron

        service cron start

        git clone https://github.com/eval2A/dionaeaToJSON.git

        mv dionaeaToJSON/dionaeaSqliteToJson.py /opt

        rm -rf dionaeaToJSON

        cd /opt

        nano dionaeaSqliteToJson.py

  Add `/lib` after `/var`

  ![alt text](image-9.png)

        crontab -e -u root

  Append this line `* * * * * /usr/bin/python3 /opt/dionaeaSqliteToJson.py` to the end of the file

  ![alt text](image-10.png)

        /opt/dionaea/bin/dionaea -l all,-debug -L '\*'

## Logs

- **Cowrie**
  ![alt text](image-11.png)

  ![alt text](image-14.png)

- **dionaea**
  ![alt text](image-12.png)

## Logs in ELK

- **Cowrie**
  ![alt text](image-15.png)

  ![alt text](image-13.png)

- **dionaea**
  ![alt text](image-16.png)
  ![alt text](image-17.png)

## Dashboards
