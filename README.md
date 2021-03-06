occi-os
=======

This is a clone and continuation of https://github.com/dizz/nova - it provides a python egg which can be easily
deployed in [OpenStack](http://www.openstack.org) and will thereby add the 3rd party [OCCI](http://www.occi-wg.org)
interface to OpenStack.

Usage
-----

1. Install this egg: `python setup.py install` (later maybe `pip install occi-os`)
2. Configure OpenStack - Add application to `api-paste.ini` of nova and enable the API

### Configuration

Make sure an application is configured in `api-paste.ini` (name can be picked yourself):

	########
	# OCCI #
	########

	[composite:occiapi]
	use = egg:Paste#urlmap
	/: occiapppipe

	[pipeline:occiapppipe]
	pipeline = authtoken keystonecontext occiapp
	# with request body size limiting and rate limiting
	# pipeline = sizelimit authtoken keystonecontext ratelimit occiapp

	[app:occiapp]
	use = egg:openstackocci#occi_app

Make sure the API (name from above) is enabled in `nova.conf`:

	[...]
	enabled_apis=ec2,occiapi,osapi_compute,osapi_volume,metadata
	[...]
	
#### Hacking the port number

You can set the port option via the `nova.conf` configuration file:

    [...]
    occiapi_listen_port=9999
    [...]
