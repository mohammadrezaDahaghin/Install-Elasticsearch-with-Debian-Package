# Install Elasticsearch with Debian Package

## **Import the Elasticsearch PGP Key**

Download and install the public signing key:

```bash
wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo gpg --dearmor -o /usr/share/keyrings/elasticsearch-keyring.gpg
```

**Installing from the APT repository**

You may need to install the `apt-transport-https` package on Debian before proceeding:

```bash
sudo apt-get install apt-transport-https
```

Save the repository definition to `/etc/apt/sources.list.d/elastic-8.x.list`:

```bash
echo "deb [signed-by=/usr/share/keyrings/elasticsearch-keyring.gpg] https://artifacts.elastic.co/packages/8.x/apt stable main" | sudo tee /etc/apt/sources.list.d/elastic-8.x.list
```

You can install the Elasticsearch Debian package with:

```bash
sudo apt-get update && sudo apt-get install elasticsearch
```

Save the security autoconfiguration information somewhere:

```bash
--------------------------- Security autoconfiguration information ------------------------------

Authentication and authorization are enabled.
TLS for the transport and HTTP layers is enabled and configured.

The generated password for the elastic built-in superuser is : CVoQUx9Cjq84zg9RH9wE

If this node should join an existing cluster, you can reconfigure this with
'/usr/share/elasticsearch/bin/elasticsearch-reconfigure-node --enrollment-token <token-here>'
after creating an enrollment token on your existing cluster.

You can complete the following actions at any time:

Reset the password of the elastic built-in superuser with
'/usr/share/elasticsearch/bin/elasticsearch-reset-password -u elastic'.

Generate an enrollment token for Kibana instances with
 '/usr/share/elasticsearch/bin/elasticsearch-create-enrollment-token -s kibana'.

Generate an enrollment token for Elasticsearch nodes with
'/usr/share/elasticsearch/bin/elasticsearch-create-enrollment-token -s node'.

-------------------------------------------------------------------------------------------------
```

You can install the kibana and logstash Debian package with:

```bash
sudo apt-get install kibana logstash
```

edit elasticsearch.yml file:

```bash
vim /etc/elasticsearch/elasticsearch.yml:
cluster.name: slavekernel
network.host: 0.0.0.0
http.port: 9200
```

edit kibana.yml:

```bash
vim /etc/kibana/kibana.yml:

server.port: 5601
server.host: "0.0.0.0"
```

start kibana and eladticsearch services:

```bash
sudo systemctl start kibana
sudo systemctl start elasticsearch
```

Now enter the server address in the browser:

```bash
http://192.168.64.16:5601/
```

Get elasticsearch token:

```bash
root@srv1:/usr/share/elasticsearch/bin# ./elasticsearch-create-enrollment-token
Creates enrollment tokens for elasticsearch nodes and kibana instances

Option (* = required)  Description
---------------------  -----------
-E <KeyValuePair>      Configure a setting
-f, --force            Use this option to force execution of the command
                         against a cluster that is currently unhealthy.
-h, --help             Show help
* -s, --scope          The scope of this enrollment token, can be either "node"
                         or "kibana"
--url                  the URL where the elasticsearch node listens for
                         connections.
-v, --verbose          Show verbose output
ERROR: Missing required option(s) [s/scope]
root@srv1:/usr/share/elasticsearch/bin# ./elasticsearch-create-enrollment-token --scope kibana
eyJ2ZXIiOiI4LjEzLjAiLCJhZHIiOlsiMTkyLjE2OC42NC4xNTo5MjAwIl0sImZnciI6IjQ4MDM0MTc4YWE2NzJmZTk5ZGViMzI1ZjZkMDFkNDExYzlhN2I4NzNjNzVkYjZkZjI5NmQ3YWEwOWI4OTZmZjMiLCJrZXkiOiJvRzJibW80QjQ4RF9mdHM1Y2tkSDpxRzJMTWxwdFQ3Mm5MUFB6aDBHOHl3In0=

```

Get the Kibana authentication code:

```bash
root@srv1:/usr/share# cd kibana/bin/kibana
kibana                    kibana-health-gateway     kibana-plugin             kibana-verification-code
kibana-encryption-keys    kibana-keystore           kibana-setup
root@srv1:/usr/share# cd kibana/bin/kibana-verification-code
bash: cd: kibana/bin/kibana-verification-code: Not a directory
root@srv1:/usr/share# cd kibana/bin/
root@srv1:/usr/share/kibana/bin# ./kibana-verification-code
Your verification code is:  055 136
```

![Screenshot 2024-04-01 203748.png](Install%20Elasticsearch%20with%20Debian%20Package%20546714499d6941a3a8feac7be3d994e2/Screenshot_2024-04-01_203748.png)