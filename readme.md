#Selenium with Galen and CBT#

Galen is a powerful automated test framework for testing and validating responsive CSS within your website or web-application. Combined with a cloud device lab like CBT, you can quickly and efficiently execute your tests on a wide array of operating systems in browsers. Here, we'll go through quickly getting set up Galen and CBT.

##Installation##

To use this integration, its easiest to go ahead and clone this repository. You can drop your test files in and quickly get them running. Navigate to the cloned directory and run:

```
npm install
```

This will install some dependencies we need in case you're interested in starting a tunnel to test locally hosted resources. If you don't already have Galen installed globally, you should do so now. I can be installed through NPM using the following command:

```
sudo npm install -g galenframework-cli
```

It can also be installed by other means, as described in [Galen's installation documentation](http://galenframework.com/docs/getting-started-install-galen/).

Once we have all the necessary dependencies installed, we can start running tests. We just need to change the value of the username and authorization key in our cbt.test file:

```
@@ set
    username     chase%40crossbrowsertesting.com
    authkey      yourauthkey												# change to your authkey
    gridCreds    ${username}:${authkey}
    gridUrl      http://${gridCreds}@hub.crossbrowsertesting.com:80/wd/hub
    website      https://app.crossbrowsertesting.com/login
    cbtCaps      --dc.record_video "true" --dc.record_network "true"

```

The username supplied would be the email address you signed up for CBT with (keep in mind the '@' character should be replaced with '%40'), and you authorization key can be found from the [Selenium Dashboard](https://app.crossbrowsertesting.com/selenium/run) portion of our site. 

The example I've supplied simply navigates to CrossBrowserTesting's login page and makes some assertions about the layout of our login form. To start up some tests, run the following command:

```
npm run test
```

Navigate over to our app, and you can see the tests being executed live. Within your terminal window, you should see test results being completed, and eventually, a passing suite:

```
========================================
Test: Login form on Safari with Wide viewport
========================================
Aug 16, 2017 10:46:52 AM org.openqa.selenium.remote.ProtocolHandshake createSession
INFO: Detected dialect: OSS
check test/loginPage.spec
= Main section =
    username:
        width ~ 340px
        height ~ 40px
        inside form

    password:
        width ~ 340px
        height ~ 40px
        inside form

    loginButton:
        width ~ 340px
        height ~ 50px

    cbtIcon:
        width ~ 255px
        height ~ 38px

    sbIcon:
        width ~	100px
        height ~ 13px


========================================
----------------------------------------
========================================
Suite status: PASS
Total tests: 8
Total failed tests: 0
Total failures: 0
```

##Changing Browsers/Viewport##

The browsers chosen by default are the latest Chrome, Firefox, IE, and Safari, but you can choose whatever you'd like. Do so by modifying the data tables in cbt.test:

```
@@ table viewports
    | viewport  | size     |
    | Narrow    | 1024x768 |
    | Wide      | 1366x768 |

@@ table desktopBrowsers
    | browser   | config             					     |
    | Firefox   | --dc.browser_api_name "firefox-latest" 	 |
    | Chrome    | --dc.browser_api_name "chrome-latest"      |
    | IE11      | --dc.browser_api_name "IE11"               |
    | Safari    | --dc.browser_api_name "Safari10"           |
```

At a minimum, we need a browser_api_name or browserName field, but youc an further specify the specific OS/Device/Browser combo with additional fields:

```
@@ table desktopBrowsers
    | browser   | config             					 								  |
    | Firefox-Mac   | --dc.browser_api_name "firefox-latest" --dc.os_api_name "Mac10.12"  |
    | Firefox-Win   | --dc.browser_api_name "firefox-latest" --dc.os_api_name "Win10"     |
```

Feel free to fill this table with whatever values you'd like! Find our API names from the configuration manager on the [Selenium Dashboard](https://app.crossbrowsertesting.com/selenium/run) of our app. 

#Parallel Execution#

The real efficiency from using a cloud service like CBT comes with parallel execution. Try running these scripts in parallel with the following command:

```
npm run test:parallel
```

This will spin up 4 tests at a time, executing continuously within that threshold until all of your test cases have completed. To modify the number, edit the package.json file or simply run the following command with the number of parallel slots allowed by your plan:

```
galen test cbt.test --parallel-tests 10
```

Now you're executing test suites with 10 times the speed you'd have had previously. 

##Support##

Galen has some great documentation out there. Everything from their API reference docs, to test suite setup documentation can be found on their site. Additionally, if you have any further questions about running tests on CBT, don't hesitate to [reach out to us](mailto:support@crossbrowsertesting.com). We're always happy to help.
