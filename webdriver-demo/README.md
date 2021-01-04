WebDriver Demo
==============

In this doc, we will see, how we can use WebDriver without language binding. We will use ChromeDriver as an example. For using it, you should have a Chrome browser installed. This demo was tested on macOS Big Sur, but with some changes should work well on other platforms.

Starting ChromeDriver
----------------------------

1. Download WebDriver, which matches your Chrome version, from https://chromedriver.chromium.org/downloads
2. Unarchive it and put it into your working directory
3. Open the terminal and navigate to the working directory
4. (Mac) As far as we download the ChromeDriver from Internet, macOS will not allow running it from the command line. So you should allow opening it first. For doing this, open the working directory in Finder, hold the **Command** button, right-click the **chromedriver** binary, and select **Open** in the context menu. After that, you'll be able to open the **chromedriver** from terminal
5. Start the **chromedriver** from the terminal. The output should have info about the **ChromeDriver** version and the port it listening to.
```bash
$ ./chromedriver
Starting ChromeDriver 87.0.4280.88 (89e2380a3e36c3464b5dd1302349b1382549290d-refs/branch-heads/4280@{#1761}) on port 9515
Only local connections are allowed.
Please see https://chromedriver.chromium.org/security-considerations for suggestions on keeping ChromeDriver safe.
ChromeDriver was started successfully.
```
6. Now we may send requests directly to the **ChromeDriver**. Let's start by opening the browser

Using ChromeDriver
-------------------------

Now, let's use the started ChromeDriver. We'll interact with it through [WebDriver Protocol](https://w3c.github.io/webdriver/) using **curl** utility. Read docs to get more info about available commands and parameters. For running commands, we should open a new terminal session.
First of all, we need to create a browser session. For doing it, we can send the following request
```bash
$ curl -q -XPOST "http://localhost:9515/session" -d \
'{
  "capabilities": {
    "firstMatch": [
      {
        "browserName": "chrome",
        "goog:chromeOptions": {
          "args": [

          ],
          "excludeSwitches": [
            "enable-automation"
          ],
          "extensions": [

          ],
          "prefs": {
            "credentials_enable_service": false,
            "profile.default_content_setting_values.notifications": 1,
            "profile.default_content_settings.popups": 0,
            "profile.password_manager_enabled": false
          }
        }
      }
    ]
  },
  "desiredCapabilities": {
    "browserName": "chrome",
    "goog:chromeOptions": {
      "args": [

      ],
      "excludeSwitches": [
        "enable-automation"
      ],
      "extensions": [

      ],
      "prefs": {
        "credentials_enable_service": false,
        "profile.default_content_setting_values.notifications": 1,
        "profile.default_content_settings.popups": 0,
        "profile.password_manager_enabled": false
      }
    }
  }
}'
```
As you can see, we send a POST to `localhost:<CHROME_DRIVER_PORT>` with the parameter with browser capabilities. In response, we should receive a JSON object. After formatting, it should look like this:
```json
{
   "value":{
      "capabilities":{
         "acceptInsecureCerts":false,
         "browserName":"chrome",
         "browserVersion":"87.0.4280.88",
         "chrome":{
            "chromedriverVersion":"87.0.4280.88 (89e2380a3e36c3464b5dd1302349b1382549290d-refs/branch-heads/4280@{#1761})",
            "userDataDir":"/var/folders/z3/2z1frs151p969nvnqcg093380000gq/T/.com.google.Chrome.SGG5VF"
         },
         "goog:chromeOptions":{
            "debuggerAddress":"localhost:52190"
         },
         "networkConnectionEnabled":false,
         "pageLoadStrategy":"normal",
         "platformName":"mac os x",
         "proxy":{

         },
         "setWindowRect":true,
         "strictFileInteractability":false,
         "timeouts":{
            "implicit":0,
            "pageLoad":300000,
            "script":30000
         },
         "unhandledPromptBehavior":"dismiss and notify",
         "webauthn:virtualAuthenticators":true
      },
      "sessionId":"<SESSION_ID>"
   }
}
```
Using **sessionId** we may now interact with our browser. For example, we may open Google page:
```bash
$ curl -q -XPOST "http://localhost:9515/session/1a8fae7618ed5d9d4c82307b3875ce8f/url" -d '{"url" : "https://www.google.com/"}'
{"value":null}
```
**ChromeDriver** allows us to work with navigation, browser windows, and frames, interact with page elements and the entire document, perform actions and handle prompts, and so on. To learn more about WebDriver commands, read [WebDriver W3C Editor's Draft](https://w3c.github.io/webdriver).
Finally, we should close browser session:
```bash
$ curl -XDELETE "http://localhost:9515/session/8ed5d9d4c82307b3875ce8f"
{"value":null}
```
After that, we may kill the ChromeDriver process.
