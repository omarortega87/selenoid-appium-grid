# selenoid-appium-grid

Selenoid running command

```bash
docker run -d --name selenoid -p 4444:4444 -v /var/run/docker.sock:/var/run/docker.sock -v $(pwd)/config:/etc/selenoid:ro -e OVERRIDE_VIDEO_OUTPUT_DIR=/home/omarortega/selenoid/video/ -v $(pwd)/logs/:/opt/selenoid/logs/ -v $(pwd)/video/:/opt/selenoid/video/ aerokube/selenoid:latest-release -log-output-dir $(pwd)/logs -service-startup-timeout 10m -session-attempt-timeout 10m
```

```bash
docker run -d --name selenoid \
-p 4444:4444 \
-v /var/run/docker.sock:/var/run/docker.sock \
-v $(pwd)/selenoid/config:/etc/selenoid:ro \
-e OVERRIDE_VIDEO_OUTPUT_DIR=/opt/selenoid/video/ \
-v $(pwd)/selenoid/logs/:/opt/selenoid/logs/ \
-v /c/Users/ingor/code/selenoid/video/:/opt/selenoid/video/ \
aerokube/selenoid:latest-release -log-output-dir /opt/selenoid/logs -service-startup-timeout 10m -session-attempt-timeout 10m
```

Selenoid UI command
```bash
docker run -d \
 --name selenoid-ui \
 -p 8080:8080 \
 --link selenoid:selenoid \
 aerokube/selenoid-ui:1.10.4 \
 --selenoid-uri "http://selenoid:4444"
```

python
```python
CAPS = {
        #'browser': 'android',
        'platformName': 'android',
        'appium:platformVersion': '10',
        'appium:automationName': 'UiAutomator2',
        #'appium:fullReset': True,
        #'appium:app': '/home/androidusr/TheApp.apk'
        'appium:deviceName': 'android',
        'enableVNC': True,
        'enableVideo': True,
        'deviceReadyTimeout': 120,
        'appium:app': 'https://github.com/appium-pro/TheApp/releases/download/v1.11.2/TheApp.apk'
        }
```

```json
{

  "android": {

    "default": "10.0",

    "versions": {

      "10.0": {

        ...

        "volumes": [

          "/home/username/app.apk:/builds/app.apk:ro"

        ]

        ...

      }

    }

  }

}
```
