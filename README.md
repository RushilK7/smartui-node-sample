<h1>Smart UI Testing With Selenium Node.JS</h1>

<img height="400" src="https://user-images.githubusercontent.com/126776938/232535511-8d51cf1b-1a33-48fc-825c-b13e7a9ec388.png">


<p align="center">
  <a href="https://www.lambdatest.com/blog/?utm_source=github&utm_medium=repo&utm_campaign=playwright-sample" target="_bank">Blog</a>
  &nbsp; &#8901; &nbsp;
  <a href="https://www.lambdatest.com/support/docs/?utm_source=github&utm_medium=repo&utm_campaign=playwright-sample" target="_bank">Docs</a>
  &nbsp; &#8901; &nbsp;
  <a href="https://www.lambdatest.com/learning-hub/?utm_source=github&utm_medium=repo&utm_campaign=playwright-sample" target="_bank">Learning Hub</a>
  &nbsp; &#8901; &nbsp;
  <a href="https://www.lambdatest.com/newsletter/?utm_source=github&utm_medium=repo&utm_campaign=playwright-sample" target="_bank">Newsletter</a>
  &nbsp; &#8901; &nbsp;
  <a href="https://www.lambdatest.com/certifications/?utm_source=github&utm_medium=repo&utm_campaign=playwright-sample" target="_bank">Certifications</a>
  &nbsp; &#8901; &nbsp;
  <a href="https://www.youtube.com/c/LambdaTest" target="_bank">YouTube</a>
</p>
&emsp;
&emsp;
&emsp;

*Learn the how to get started with Smart UI testing with Selenium Node.JS on the LambdaTest platform.*


[<img height="58" width="200" src="https://user-images.githubusercontent.com/70570645/171866795-52c11b49-0728-4229-b073-4b704209ddde.png">](https://accounts.lambdatest.com/register?utm_source=github&utm_medium=repo&utm_campaign=playwright-sample)


## Getting Started with Smart UI Testing

Smart UI testing is an integral part of ensuring visual consistency across different environments for your web application. Using the LambdaTest platform with Selenium and Node.js, this process becomes intuitive and efficient.

### Pre-Requisites: 

To execute the tests, your need to login to your lambdatest account and from the `Sidebar` you can find the `Username` and `Access Key` information which needs to be added to your environment variables: 

For MacOS/Linux: 

```bash
export LT_USERNAME="<Your Username>"
export LT_ACCESS_KEY="<Your Access Key>"
```

For Windows CMD

```bash
set LT_USERNAME="<Your Username>"
set LT_ACCESS_KEY="<Your Access Key>"
```

For Windows PowerShell

```bash
$env:LT_USERNAME="<Your Username>"
$env:LT_ACCESS_KEY="<Your Access Key>"
```

Now, navigate to `SmartUI` section from the sidebar and create a new project with the `project type` as the following: 

- **Web** - For running the tests using `hooks` within the `selenium/cypress/playwright` functional tests.
- **CLI** - For running your `SDK` execution for DOM capture and render on multiple browsers and viewports for comparison.

### Features

#### LT-Hooks Integration
- **Location:** See the `hooks` folder, where you can see all the `examples` scripts to setup your suite or run the demo.  
- **Purpose:** Enhance visual regression capabilities in your LambdaTest web automation tests.
- **Benefits:** Increase efficiency with advanced testing features with visual regression testing.
- **Documentation:** [LambdaTest Selenium Visual Regression Documentation](https://www.lambdatest.com/support/docs/selenium-visual-regression/).
##### **Steps:**
- Install the dependencies.
```bash
cd hooks
npm i
```
- Configure the capabilities (SmartUI Project Name and other SmartUI options) in .examples\test.js
```javascript
let capabilities = {
  platform: "Windows 10", // Configure your OS for Selenium test
  browserName: "chrome", // Configure your Browser for Selenium test
  version: "latest", // Configure your Browser Version for Selenium test
  visual: true, // Configure your Capture screenshot  for Selenium test
  name: "test session", // name of the test for Selenium
  build: "Automation Build", // name of the build for Selenium
  "smartUI.project": "<Your Project Name>", // Replace the name of project with the new project name
  "smartUI.build": "<Your Build Name>", // Replace the name of Build with the new Build name
  "smartUI.baseline": false, // Enable if you want to update to a new baseline build

};
```
- Add the Screenshot hook in .examples\test.js
```js
let config = {
  screenshotName: '<Name of your screenshot>'
};
await driver.executeScript("smartui.takeScreenshot", config);
```
- Run the script
```
node .examples\test.js
```


#### SmartUI SDK Utilization
- **Location:** Check out the `sdk` folder, and setup the environment for running the tests. 
- **Purpose:** Use the SmartUI SDK for comprehensive visual testing in both local and remote automation tests environments.
- **Advantages:** Ensure consistent DOM capturing and rendering across various browsers and resolutions.
- **More Information:** [SmartUI Selenium JavaScript SDK Documentation](https://www.lambdatest.com/support/docs/smartui-selenium-js-sdk/).

##### Steps:
- Install the dependencies 
```bash
cd sdk
npm i @lambdatest/smartui-cli @lambdatest/selenium-driver selenium-webdriver
```
- Configure your Project Token

Setup your project token show in the **SmartUI** app after, creating your project.

<Tabs className="docs__val" groupId="language">
<TabItem value="MacOS/Linux" label="MacOS/Linux" default>

```bash
export PROJECT_TOKEN="123456#1234abcd-****-****-****-************"
```

</TabItem>
<TabItem value="Windows" label="Windows - CMD">

```bash
set PROJECT_TOKEN="123456#1234abcd-****-****-****-************"
```

</TabItem>
<TabItem value="Powershell" label="Windows-PS">

```bash
$Env:PROJECT_TOKEN="123456#1234abcd-****-****-****-************"
```
</TabItem>
</Tabs>

- Add the SmartUI function to take screenshot to `sdkLocal.js` file
```js
import  { Builder, By, Key, until } from 'selenium-webdriver';
import { smartuiSnapshot } from '@lambdatest/selenium-driver';

(async function example() {
    let driver = await new Builder()
        .forBrowser('chrome')
        .build();

    try {
        await driver.get('<Required URL>'); //enter your desired URL here
        await smartuiSnapshot(driver, '<Screenshot_Name>');
    } finally {
        await driver.quit();
    }
})();
```
- Execute the Tests on SmartUI Cloud

**Local Execution:**
```bash
npx smartui exec node sdkLocal.js
```

**Cloud Execution:**
```bash
npx smartui exec node sdkCloud.js
```