# onkyo-api

Use a Raspberry Pi Zero to expose an API to my Onkyo Receiver for an iOS app.

## Install

```sh
$ python -m venv env
$ source env/bin/activate
$ pip install -r requirements.txt
```

## Run
```sh
$ ./app.py
```

## Example Requests
### Aggregates the results of several commands into an easy status response
GET `/onkyo/status`
```sh
$ curl "http://localhost:8080/onkyo/status"
```

```json
{
  "status": {
    "main": {
      "source": "tv",
      "status": "on",
      "volume": 55
    },
    "zone2": {
      "source": "appletv",
      "status": "standby",
      "volume": 0
    }
  }
}
```

### Controlling power status
PUT `/onkyo/<zone>/power/<value>`
```sh
$ curl -X "PUT" "http://localhost:8080/onkyo/main/power/on"
$ curl -X "PUT" "http://localhost:8080/onkyo/main/power/standby"
$ curl -X "PUT" "http://localhost:8080/onkyo/zone2/power/on"
$ curl -X "PUT" "http://localhost:8080/onkyo/zone2/power/standby"
```

### Controlling volume
PUT `/onkyo/<zone>/volume/<level>`
```sh
$ curl -X "PUT" "http://localhost:8080/onkyo/main/volume/55"
```

```json
{
  "volume": 55,
  "zone": "main"
}
```

## Scheduled task
Run on startup on the Raspberry Pi as a task in crontab
```sh
$ crontab -e # edit cron table

@reboot /home/pi/.pyenv/shims/python3 /home/pi/bin/onkyo-server/app.py > /home/pi/logs/onkyo-server.log 2>&1 &
```

## Author

Daniel Bowden

[github.com/danielbowden](https://github.com/danielbowden)

[twitter.com/danielgbowden](https://twitter.com/danielgbowden)
