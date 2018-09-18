# Curl Scripts

The scripts in this directory show the basics of accessing
the Metasys Server API from the command line using `curl`.

These scripts have the following dependencies:

* curl - standard on mac, unix and linux. Available for Windows
  thru Windows Subsystem for Linux (WSL), cygwin or chocolatey)
* [jq](https://stedolan.github.io/jq/download/) - A command line library for processing  JSON.

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

You can make requests of Metasys right from the command line given you have `curl` and`jq`.

This example will show you how to get an access token and fetch the first page of devices.

### Login

To get a token you must pass some credentials. The easiest way to do this is to create a file
that contains your username and password and then use `curl` to pass this file to the login endpoint.

**Warning** Be sure to protect access to this file since it contains your credentials. While this
method works for educational purposes it has security concerns. Consider using a credential store
to securely store this information.

```json
{
  "username": "johnsmith",
  "password": "xj38sj9##Abo-y"
}
```

Assuming this file was named `credentials.json` you can get an access token with this command.
(Note: The accessToken shown has been truncated for sake of space.)

```bash
$ curl --data-binary @credentials.json -L -H "Content-Type application/json" https://hostname/api/v1/login
{"accessToken":"eyJ0eXAiOiJKV1QiLCJhbGciOiJ...","expires":"2018-09-17T19:38:58Z"}
```

**Warning** Be sure to protect your access tokens as they grant anyone who has one access to Metasys.

*Explanation* We have used the `--data-binary` option to specify a file that contains the payload we need to send to the server. The `-L` option instructs curl to follow redirects. The `-H` option specifies a `Content-Type` header
which is required so the server know how to interpret the data we send it.

### Use Access Token to Access a Resource

Here's how you might use this access token to access one of the Metasys® APIs:

```bash
curl -L -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJ..." https://hostname/api/v1/networkDevices
```

We are using the `-H` option to specify an `Authorization` header which is used to pass our access token.

### Putting it all together

To avoid having to copy/paste the access_token we can use `jq` to extract the `accessToken` out of the
login request and store it in a variable. This variable can then be used to pass the `accessToken`
to the next call.

```json
TOKEN=$(curl --data-binary @credentials.json -L -H "Content-Type: application/json" https://hostname/api/v1/login | jq -r .accessToken)
curl -L -H "Authorization: $TOKEN" https://hostname/api/v1/networkDevices
```

Let's break down what is happening in the first call: 