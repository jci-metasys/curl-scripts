# Devices Script

This is a bash script that will log in and fetch the first page of devices.

It will prompt you for username, password and host name. If successful, it will login
and fetch the first page of devices. In addition to writing them to the console they will
be stored in a file named `output/devices.json`.

```shell
$ ./devices
username: john
password:
host: pc1
Logging In

Fetching devices
{
  "total": 12,
  "next": null,
  "previous": null,
  "items": [
    {
      "id": "f55a7799-ec10-5361-8569-04258bdd8070",
      "itemReference": "pc1:NAE5510",
      "name": "NAE5510",
      "type": "/enumSets/508/members/185",
      "description": "An engine",
      "firmwareVersion": "8.0.0.0449",
      "category": "/enumSets/33/members/1",
      "timeZone": "/enumSets/576/members/53",
      "self": "/networkDevices/f55a7799-ec10-5361-8569-04258bdd8070",
      "parent": null,
      "networkDevices": "/networkDevices/f55a7799-ec10-5361-8569-04258bdd8070/networkDevices",
      "equipment": "/networkDevices/f55a7799-ec10-5361-8569-04258bdd8070/equipment",
      "spaces": "/networkDevices/f55a7799-ec10-5361-8569-04258bdd8070/spaces",
      "objects": "/networkDevices/f55a7799-ec10-5361-8569-04258bdd8070/objects",
      "attributes": "/networkDevices/f55a7799-ec10-5361-8569-04258bdd8070/attributes",
      "alarms": "/networkDevices/f55a7799-ec10-5361-8569-04258bdd8070/alarms"
    },
    // Other results not shown
}
```

## Output

If everything works correctly, the `output` directory will contain 5 files:

```shell
$ ls output
access_token.txt	devices.json		login-result.json
devices-headers.txt	login-headers.txt
```

Here is a description of each file

| File                | Description                                                    |
| ------------------- | -------------------------------------------------------------- |
| login-headers.txt   | The headers from the login request, if headers were received.  |
| login-result.json   | The response from the login request, if any was received.      |
| access_token.txt    | The access_token extracted from the login-result.json file     |
| devices-headers.txt | The headers from the devices request, if headers were received |
| devices.json        | The response from the devices request, if any was received     |

If something goes wrong these files will contain the headers and response bodies to help you
debug.