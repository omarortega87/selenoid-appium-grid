# selenoid-appium-grid

Selenoid running command

```bash
docker run -d --name selenoid -p 4444:4444 -v /var/run/docker.sock:/var/run/docker.sock -v $(pwd)/config:/etc/selenoid:ro -e OVERRIDE_VIDEO_OUTPUT_DIR=/home/omarortega/selenoid/video/ -v $(pwd)/logs/:/opt/selenoid/logs/ -v $(pwd)/video/:/opt/selenoid/video/ aerokube/selenoid:latest-release -log-output-dir $(pwd)/logs -service-startup-timeout 10m -session-attempt-timeout 10m
```
## Docker Selenoid Server
```bash
docker run -d --name selenoid \
-p 4444:4444 \
-v /var/run/docker.sock:/var/run/docker.sock \
-v $(pwd)/selenoid/config:/etc/selenoid:ro \
-e OVERRIDE_VIDEO_OUTPUT_DIR=/opt/selenoid/video/ \
-v $(pwd)/selenoid/logs/:/opt/selenoid/logs/ \
-v $(pwd)/selenoid/video/:/opt/selenoid/video/ \
aerokube/selenoid:latest-release -log-output-dir /opt/selenoid/logs -service-startup-timeout 10m -session-attempt-timeout 10m
```

## Selenoid UI command
```bash
docker run -d \
 --name selenoid-ui \
 -p 8080:8080 \
 --link selenoid:selenoid \
 aerokube/selenoid-ui:1.10.4 \
 --selenoid-uri "http://selenoid:4444"
```

## Docker Selenoid Video image
```bash
docker pull selenoid/video-recorder:latest-release 
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


```java
package tests;

import io.appium.java_client.AppiumBy;
import io.github.bonigarcia.wdm.WebDriverManager;
import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.remote.DesiredCapabilities;
import org.openqa.selenium.remote.RemoteWebDriver;
import org.openqa.selenium.support.ui.ExpectedCondition;
import org.openqa.selenium.support.ui.ExpectedConditions;
import org.openqa.selenium.support.ui.WebDriverWait;
import org.testng.annotations.AfterMethod;
import org.testng.annotations.BeforeMethod;
import org.testng.annotations.Test;
import io.appium.java_client.android.AndroidDriver;

import java.io.File;
import java.net.MalformedURLException;
import java.net.URL;
import java.time.Duration;
import java.util.ArrayList;
import java.util.HashMap;

import static org.assertj.core.api.Assertions.assertThat;

public class AppiumRaw {

    RemoteWebDriver driver;

    @BeforeMethod
    public void setup() {

        DesiredCapabilities capabilities = new DesiredCapabilities();

        capabilities.setCapability("appium:platformVersion", "10");
        capabilities.setCapability("appium:deviceName", "android");
        capabilities.setCapability("appium:automationName", "UiAutomator2");
        capabilities.setCapability("appium:app", "https://github.com/appium-pro/TheApp/releases/download/v1.11.0/TheApp.apk");
        capabilities.setCapability("selenoid:options", new HashMap<String, Object>() {{
            /* How to add test badge */
            put("name", "Test badge...");

            /* How to set session timeout */
            put("sessionTimeout", "15m");

            /* How to set timezone */
            put("env", new ArrayList<String>() {{
                add("TZ=UTC");
            }});

            /* How to add "trash" button */
            put("labels", new HashMap<String, Object>() {{
                put("manual", "true");
            }});

            /* How to enable video recording */
            put("enableVNC", true);
            put("enableVideo", true);
            put("videoName", "my-cool-video.mp4");
        }});

        // Open the app.
        try {
            driver = new RemoteWebDriver(new URL("http://localhost:4444/wd/hub"), capabilities);
        } catch (MalformedURLException e) {
            throw new RuntimeException(e);
        }

    }

    @AfterMethod
    public void teardown() {
        driver.quit();
    }

    @Test(alwaysRun = true)
    public void testVanillaBasicLoginSuccess() {
        WebDriverWait wait = new WebDriverWait(driver, Duration.ofSeconds(10));
        wait.until(ExpectedConditions.presenceOfElementLocated(AppiumBy.accessibilityId("Login Screen"))).click();
    }

}

```
