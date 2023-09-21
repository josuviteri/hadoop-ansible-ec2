# Ansible script for deploying Hadoop instaces at AWS EC2

1. Set AWS credentials at ~/.aws/credentials
2. Download SSH private key at ~/.ssh/vockey.pem
3. Set the proper permissions for the key: `chmod og-rwx ~/.ssh/vockey.pem`
4. Install requirements: `pip install -r requirements.txt`
5. Install vim and less: `sudo apt update && apt install -y vim less`
6. Create instances: `ansible-playbook create-instances.yml`
7. Edit `inventory.yml` file and replace each server **public** DNS with the current ones.
8. Install Hadoop: `ansible-playbook -i inventory.yml --key-file=~/.ssh/vockey.pem --user ec2-user install-hadoop.yml`
9. Edit hadoop-master/workers file setting the actual workers **private** DNS.
10. Edit core-site.xml and set the actual master **private** DNS at `fs.defaultFS` property.
11. Configure hadoop nodes: `ansible-playbook -i inventory.yml --key-file=~/.ssh/vockey.pem --user ec2-user configure-hadoop.yml`