# Ansible script for deploying Hadoop instaces at AWS EC2

1. Set AWS credentials at ~/.aws/credentials
2. Download SSH private key at ~/.ssh/vockey.pem
3. Set the proper permissions for the key: `chmod og-rwx ~/.ssh/vockey.pem`
4. Install requirements: `pip install -r requirements.txt`
5. Install vim and less: `sudo apt update && sudo apt install -y vim less`
6. Create instances: `ansible-playbook create-instances.yml`
7. Edit `inventory.yml` file and replace each server **public** DNS with the current ones.
8. Install Hadoop: `ansible-playbook -i inventory.yml --key-file=~/.ssh/vockey.pem --user ec2-user install-hadoop.yml`
9. Edit hadoop-master/workers file setting the actual workers **private** DNS.
10. Edit core-site.xml and set the actual master **private** DNS at `fs.defaultFS` property.
11. Configure hadoop nodes: `ansible-playbook -i inventory.yml --key-file=~/.ssh/vockey.pem --user ec2-user configure-hadoop.yml`


# Install YARN

1. Edit `inventory.yml` file and replace each server **public** DNS with the current ones.
2. Edit hadoop-worker/yarn-site.xml and set hadoop-master's **private** DNS at yarn.resourcemanager.hostname property.
3. Edit hadoop-client/yarn-site.xml and set hadoop-master's **private** DNS at yarn.resourcemanager.hostname property.
4. Configure YARN: `ansible-playbook -i inventory.yml --key-file=~/.ssh/vockey.pem --user ec2-user install-yarn.yml`
5. To connect to the ResourceManager Web UI, open a ssh tunnel to the master node and access to http://ec2-44-192-103-94.compute-1.amazonaws.com:8088 (replace by your hadoop-master public IP): 
```
ssh -i ~/.ssh/vockey.pem -N -L 8088:ec2-44-192-103-94.compute-1.amazonaws.com:8088 ec2-user@ec2-44-192-103-94.compute-1.amazonaws.com
```
6. To launch wordcount example, connecto to hadoop-client and run:
```
hadoop-3.3.6/bin/yarn jar hadoop-3.3.6/share/hadoop/mapreduce/hadoop-mapreduce-examples-3.3.6.jar wordcount </input-dir> </output-dir>
```
