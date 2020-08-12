purei9-cli
==========

Small proof-of-concept client to connect to Electrolux and AEG cleaner robots.

Usage
-----

First you need to get your local robot pw to talk to the robot.

	$ ./purei9-cli.py cloud user@email.com mypassword
	[
		{
			"RobotID": "900395798357985798375972",
			"Connected": true,
			"FirmwareVersion": "40.17",
			...
			"LocalRobotPassword": "29379204",
			...
		}
	]
	
Now you can connect to your robot.

	$ ./purei9-cli.py local 192.168.1.101 29379204 status
	
	 [<] Connecting to 192.168.1.101:3002
	 [>] Connnected
	 [i] Server Cert
	-----BEGIN CERTIFICATE-----
	...
	-----END CERTIFICATE-----
	...
	 [>] recv 3009 user1=0 user2=0 len=47
	{
		"id": "900395798357985798375972",
		"name": "Cleaner",
		"status": "Sleeping",
		"settings": {
			"EcoMode": false,
			"Language": "eng",
			"Mute": false
		}
	}

More usage:

	Usage: ./purei9-cli.py [cloud <email> <password>]
	       ./purei9-cli.py [local <address> <localpw> [status|firmware|start|home]]

		cloud: connect to purei9 cloud to get your localpw

		local: connect to robot at <address> using <localpw>
			status   - show basic status
			firmware - show firmware info
			start    - start cleaning
			home     - stop cleaning and go home

