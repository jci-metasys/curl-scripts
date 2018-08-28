# Curl Scripts

The scripts in this directory show the basics of accessing
the Metasys Server API from the command line using `curl`.

These scripts have the following dependencies:

* curl - standard on mac, unix and linux. Available for Windows
  thru cygwin or chocolatey)
* [jq](https://stedolan.github.io/jq/download/) - A command line javascript 

The following scripts exist

* login - simply logs in and writes the access token to the file
token.

* devices - logs in and retrieves the first pages of network devices.
Two files are written
    - networkDevices.json - The first page of devices
    - abbreviatedDevices.json - The first page of devices with only
      the fields id, name, description, and itemReference for each
      devices.
