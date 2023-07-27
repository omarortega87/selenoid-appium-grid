# selenoid-appium-grid

Selenoid running command

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
