#working with libs

1. 
When working on OS, you are required to get all possible info regarding the os you are working on. by using libraries os, sys, netifaces,ipaddress, platform, subprocess. please provide data about:
	*interface
	*network shares
	*users
	*storage
	*filesystem
	*system variables
	*python version
	*kernel version
	
2.
As part system application automation, you are required to install and configure nosql library called tinyDB.
the steps of linrady install, deploy and confifuration are:
	* install tinydb python library.
	* import tinydb  library and creaty basic json file to store data in it.
	* validate that json file is query is empty.
Create an automated python script that will do all steps above and will use all possible libraries: os,sys,time,json
tinydb,subprocess,string.

https://tinydb.readthedocs.io/en/latest/getting-started.html#basic-usage

3.
as part of network automation, your task is to install, manage and deploy servers by using paramiko library.
create a script that will ask you destination server, remote server command and user/password for remote connection.
it will connect to server and return hostname the output to user.
				https://www.youtube.com/watch?v=kvPa85M9z2Q&pbjreload=10


1.
import os
import sys
import platform
try:
	import netifaces as ni
except ModuleNotFoundError:
	os.system('sudo pip3 install netifaces')
except Exception as e:
	print('Unable to install netifaces -  try installing manually')
	sys.exit(1)
try:
	import ipaddress
except ModuleNotFoundError:
	os.system('sudo pip3 install ipaddress')
except Exception as e:
	print('Unable to install netifaces -  try installing manually')
	sys.exit(1)


def get_inet_details(x): #NEED TO SOLVE
	inet=ni.ifaddresses(x)
	while inet is not None:
		
	return inet
	


for i in ni.interfaces():
	print(i,get_inet_details(i))
	
	

profile = [
platform.architecture(),
platform.dist(),
platform.libc_ver(),
platform.mac_ver(),
platform.node(),
platform.platform(),
platform.processor(),
platform.python_build(),
platform.python_compiler(),
platform.python_version(),
platform.system(),
#platform.uname(),
platform.version(),
]

for p in profile:
	print(p)




2.


import os
import sys
import time

try: 

	from tinydb import TinyDB, Query


except ModuleNotFoundError:
	print('Module is not installed: Trying to install ')
	time.sleep(1)
	os.system('pip3 install --user tinydb')
	

except Exception as e:
	print(e)
	print('try installing manually')
	sys.exit(1)



db = TinyDB('db.json')

def db_exits(path='.',db_name):
	if os.path.exists(db_name):
		return True
	return False

if __name__ == "__main__":
	db_exits(db)
	if db.all() == []:
		print('DB exists and is expty')
		sys.exit(0)
	else:
		print('DB not found')
		sys.exit(1)




3.
Need to verify
import os
import sys
import time

try:
	import paramiko

except Exception as e:
	print('lib missing: try installing it manually')
	sys.exit(1)
	
def ssh_connect(dest='', port=22,username='',passwd=''):
	if dest == '':
		dest = input('destination missing. Please insert ip: ') 
	if username == '':
		username = input('connection input missing: username:')	
	if passwd == '':
		passwd = input('password missing : provide password: ')
	if port == 22:
		ans = input('port is 22: change ? y/N')
		if ans == 'y' or ans == 'Y':
			port = int(input('Please provide port: '))
		else:
			port =22
	try:
		ssh =  paramiko.SSHClient()
		ssh.load_system_host_keys()
		ssh.connect(dest,username=username,password=passwd, port=port)
	except SSHException:
		ssh.set_missing_host_key_policy(paramiko.AutoAddPolicy())
	#except Exception as e:
	#	print('error',e,'occured')
	#	sys.exit(1)

	stdin,stdout,stderr = ssh.exec_command('ls -la')
	output=stdout.readlines()
	return output
	
if __name__ == "__main__":
	var=ssh_connect()
	print(var)



if __name__ == "__main__":
	ssh_connect()
