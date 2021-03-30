purei9-cli
==========

Small proof-of-concept client to connect to Electrolux and AEG cleaner robots.

Compatibility
-------------

Only tested with an AEX RX9 (aka purei9) first Generation.

Update: Also seems to work with Purei9.2: https://community.home-assistant.io/t/integrating-eectrolux-pure-i9-robotic-vacuum/78648/11

Security
--------

Other than the purei9 app, this tool does not verify the robot's TLS certificate, so beware of MitMs in your LAN, eavedropping on your robot. In case you are curious how the trust model works anyway: The TLS certificate of the robot is self signed and verified against a known public key which is gathered from the purei9 cloud.

Disclaimer
----------

The developer of this software is not affiliated at all with Electrolux. Electrolux, AEG and Purei9 are brand/product names by Electrolux AB which i do not have any rights upon.

Usage
-----

First you need to get your local robot pw to talk to the robot.

	$ ./purei9.py cloud user@email.com mypassword
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
	
You can also use the tool to locate any robots in the network

	$ python3 purei9.py search
	+---------------+--------------------------+---------+
	|   Address     |         RobotID          |  Name   |
	+---------------+--------------------------+---------+
	| 192.168.1.101 | 900395798357985798375972 | Cleaner |
	+---------------+--------------------------+---------+
	
Now you can connect to your robot.

	$ python3 purei9.py local 192.168.1.101 29379204 status
	
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

	Usage: purei9.py [cloud <email> <password>] [status]
	       purei9.py [cloud <email> <password>] maps <robotid> [write_files]
	       purei9.py [local <address> <localpw> [status|firmware|start|home]]
	       purei9.py [search]

	    cloud: connect to purei9 cloud to get your localpw (does not work currently)

	    local: connect to robot at <address> using <localpw>
		   status   - show basic status
		   firmware - show firmware info
		   start    - start cleaning
		   home     - stop cleaning and go home

	    search: search for robots in the local network

