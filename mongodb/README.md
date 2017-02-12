Deploying a sharded MongoDB(3.2+) cluster with Ansible(2.1+)
------------------------------------------------------------------------------

- Requires Ansible 2.1+ 
- Expects CentOS/RHEL 7 hosts

 
### Deployment Example
------------------------------------------------------------------------------

The inventory file looks as follows:

		#The site wide list of mongodb servers
		[mongo_servers]
		mongo1 mongod_port=2700
		mongo2 mongod_port=2701
		mongo3 mongod_port=2702

		#The list of servers where replication should happen, including the master server.
		[replication_servers]
		mongo3
		mongo1
		mongo2

		#The list of mongodb configuration servers, make sure it is 1 or 3
		[mongoc_servers]
		mongo1
		mongo2
		mongo3

		#The list of servers where mongos servers would run. 
		[mongos_servers]
		mongos1
		mongos2

Build the site by mongodb playbook using the following command:

		ansible-playbook -i hosts site.yml
