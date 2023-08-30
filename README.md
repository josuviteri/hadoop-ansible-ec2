# Ansible script for deploying Hadoop instaces at AWS EC2

1. Set AWS credentials at ~/.aws/credentials
2. Download SSH private key at ~/.ssh/vockey.pem
3. Install requirements: `pip install -r requirements.txt`
4. Install vim and less: `sudo apt update && apt install -y vim less`
5. Run instances: `ansible-playbook create-instances.yml`
5. Edit `inventory.yml` file and replace each server **public** DNS with the current ones.
6. Install Hadoop: `ansible-playbook -i inventory.yml --key-file=~/.ssh/vockey.pem --user ec2-user install-hadoop.yml`
7. Edit hadoop-master/workers file setting the actual workers **private** DNS.
8. Edit hadoop-master/hdfs-site.xml and set the actual master **private** DNS at `fs.defaultFS` property.
9. Configure hadoop-master node: `ansible-playbook -i inventory.yml --key-file=~/.ssh/vockey.pem --user ec2-user configure-master.yml`
10. Edit hadoop-worker/hdfs-site.xml and set the actual master **private** DNS at `fs.defaultFS` property.
11. Configure hadoop-worker nodes: `ansible-playbook -i inventory.yml --key-file=~/.ssh/vockey.pem --user ec2-user configure-worker.yml`