# Curl Scripts

The scripts in this directory show the basics of accessing
the Metasys Server API from the command line using `curl`.

These scripts have the following dependencies:

* curl - standard on mac, unix and linux. Available for Windows
  thru Windows Subsystem for Linux (WSL), cygwin or chocolatey)
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

## Getting Started with Curl

You can make request of Metasys right from the command line given you have `curl` and`jq`.

This example will show you how to get an access token and fetch the first page of devices.

To get a token you must pass some credentials. The easiest way to do this is to create a file
that contains your username and password and then use `curl` to pass this file. (**Note** Be sure to
protect access to this file since it contains your credentials.)

```json
{
  "username": "johnsmith",
  "password": "xj38sj9##Abo-y"
}
```

Assuming this file was named `credntials.json` you can get an access token with this command.
(Note: The accessToken shown has been truncated for sake of space.)

```bash
$ curl --data-binary @credentials.json -L -H "Content-Type application/json" https://hostname/api/v1/login
{"accessToken":"eyJ0eXAiOiJKV1QiLCJhbGciOiJ...","expires":"2018-09-17T19:38:58Z"}
```

Here's how you might use this access token to access one of the Metasys® APIs:

```bash
curl -L -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJ..." https://hostname/api/v1/networkDevices
```

To avoid having to copy/paste the access_token we can use `jq`:

```json
$ TOKEN=$(curl --data-binary @credentials.json -L -H "Content-Type: application/json" https://hostname/api/v1/login | jq -r .accessToken)
$ curl -L -H "Authorization: $TOKEN" https://hostname/api/v1/networkDevices
{"id":"f55a7799-ec10-5361-8569-04258bdd8070","itemReference":"hostName:naeName","name":"naeName","type":"/enumSets/508/members/185","description":"Auth Cat Fire","firmwareVersion":"8.0.0.0449","category":"/enumSets/33/members/1","timeZone":"/enumSets/576/members/53","self":"/networkDevices/f55a7799-ec10-5361-8569-04258bdd8070","parent":null,"networkDevices":"/networkDevices/f55a7799-ec10-5361-8569-04258bdd8070/networkDevices","equipment":"/networkDevices/f55a7799-ec10-5361-8569-04258bdd8070/equipment","spaces":"/networkDevices/f55a7799-ec10-5361-8569-04258bdd8070/spaces","objects":"/networkDevices/f55a7799-ec10-5361-8569-04258bdd8070/objects","attributes":"/networkDevices/f55a7799-ec10-5361-8569-04258bdd8070/attributes","alarms":"/networkDevices/f55a7799-ec10-5361-8569-04258bdd8070/alarms"}
```