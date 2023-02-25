# maven-project

Simple Maven Project
First create 2 servers ansible and tomcat server.
Create password less authuntication between ansible server and tomcate server using ssh-keygen cmd on ansible server.
and under cd /root/.ssh/ copy the id_rsa.pub key and do the ssh-keygen on tomcat server and under same directory past the copied key in authorized_keys
after try to ping from ansible server to tomcat using ssh 10.100.8.0 (private ip of tomcat server)
and under ansble server cd /etc/ansible under that hosts file past the private ip address of tomcat so that they both can talk.
# configuring on Jenkins 

In order to deploy on war file from ansible to tomcat server we need do install one plugin called publish over ssh.
Now go to jenkins in manage jenkins in Configure System there will option called ssh server.
Under ssh server give name as ansible and Hostname as tomcate private address and username as ec2-user 
under that there will one option advansed check box the Use password authentication, or use a different key under that key option 
paste the .pem key content and test the confuguration and should get success msg and yor done.
Now create job and configure the job under post build step add Send files or execute commands over SSH and configure.
Provide SSH server which will autopopulate and source files give webapp/target/*.war and and remote directory //opt//playbooks and Exec command as ansible-playbook /opt/playbooks/copyfile.yml
which bascially means copy war and past on ansible server under cd /opt/playbooks/webapp/target/webapp.war and past it on tomcat server by executing playbook  specified
and this how playbooks looks like 
---
- hosts: all
  become: true
  tasks:
    - name: copy war onto tomcat server
      copy:
        src: /opt/playbooks/webapp/target/webapp.war
        dest: /opt/tomcat/webapps
        
This is say copu war to ansible which is src and from there to destination to tomcat under dest path and your war file be placed
and you run build the job and your done
