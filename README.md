# SmartUI SDK Sample for Selenium JavaScript

Welcome to the SmartUI SDK sample for Selenium JavaScript. This repository demonstrates how to integrate SmartUI visual regression testing with Selenium JavaScript.

## Repository Structure

```
smartui-node-sample/
├── sdk/
│   ├── sdkCloud.js          # Cloud test
│   ├── sdkLocal.js          # Local test
│   └── smartui-web.json     # SmartUI config (create with npx smartui config:create)
└── hooks/                    # Hooks integration examples
    └── examples/             # Hooks examples
```

## 1. Prerequisites and Environment Setup

### Prerequisites

- Node.js installed
- LambdaTest account credentials (for Cloud tests)
- Chrome browser (for Local tests)

### Environment Setup

**For Cloud:**
```bash
export LT_USERNAME='your_username'
export LT_ACCESS_KEY='your_access_key'
export PROJECT_TOKEN='your_project_token'
```

**For Local:**
```bash
export PROJECT_TOKEN='your_project_token'
```

## 2. Initial Setup and Dependencies

### Clone the Repository

```bash
git clone https://github.com/LambdaTest/smartui-node-sample
cd smartui-node-sample/sdk
```

### Install Dependencies

Install the required dependencies:

```bash
npm i @lambdatest/smartui-cli @lambdatest/selenium-driver selenium-webdriver
```

**Dependencies included:**
- `@lambdatest/smartui-cli` - SmartUI CLI
- `@lambdatest/selenium-driver` - SmartUI SDK for Selenium JavaScript
- `selenium-webdriver` - Selenium WebDriver

**Note**: To ensure seamless execution of ES6 modules, add `"type": "module"` to your `package.json` file.

### Create SmartUI Configuration

```bash
npx smartui config:create smartui-web.json
```

## 3. Steps to Integrate Screenshot Commands into Codebase

The SmartUI screenshot function is already implemented in the repository.

**Cloud Test** (`sdk/sdkCloud.js`):
```javascript
import { smartuiSnapshot } from '@lambdatest/selenium-driver';

await driver.get('https://www.lambdatest.com');
await smartuiSnapshot(driver, "screenshot");
```

**Local Test** (`sdk/sdkLocal.js`):
```javascript
import { smartuiSnapshot } from '@lambdatest/selenium-driver';

await driver.get('https://www.lambdatest.com');
await smartuiSnapshot(driver, "screenshot");
```

**Note**: The code is already configured and ready to use. You can modify the URL and screenshot name if needed.

## 4. Execution and Commands

### Local Execution

```bash
npx smartui exec node sdkLocal.js
```

### Cloud Execution

```bash
npx smartui exec node sdkCloud.js
```

## Testing with LambdaTest Hooks

This repository also includes examples for using SmartUI with LambdaTest Hooks integration.

### Hooks Integration

**Location:** See the `hooks` folder, where you can see all the `examples` scripts to setup your suite or run the demo.

**Purpose:** Enhance visual regression capabilities in your LambdaTest web automation tests.

**Documentation:** [LambdaTest Selenium Visual Regression Documentation](https://www.lambdatest.com/support/docs/selenium-visual-regression/).

### Hooks Setup Steps

1. Install the dependencies:
```bash
cd hooks
npm i
```

2. Configure the capabilities (SmartUI Project Name and other SmartUI options) in `examples/test.js`:
```javascript
let capabilities = {
  platform: "Windows 10",
  browserName: "chrome",
  version: "latest",
  visual: true,
  name: "test session",
  build: "Automation Build",
  "smartUI.project": "<Your Project Name>",
  "smartUI.build": "<Your Build Name>",
  "smartUI.baseline": false
};
```

3. Add the Screenshot hook in `examples/test.js`:
```javascript
let config = {
  screenshotName: '<Name of your screenshot>'
};
await driver.executeScript("smartui.takeScreenshot", config);
```

4. Run the script:
```bash
node examples/test.js
```

## Test Files

### Cloud Test (`sdk/sdkCloud.js`)

- Connects to LambdaTest Cloud using Selenium Remote WebDriver
- Reads credentials from environment variables (`LT_USERNAME`, `LT_ACCESS_KEY`)
- Takes screenshot with name: `screenshot`

### Local Test (`sdk/sdkLocal.js`)

- Runs Selenium locally using Chrome
- Requires Chrome browser installed
- Takes screenshot with name: `screenshot`

## Configuration

### SmartUI Config (`smartui-web.json`)

Create the SmartUI configuration file using:
```bash
npx smartui config:create smartui-web.json
```

## View Results

After running the tests, visit your SmartUI project dashboard to view the captured screenshots and compare them with baseline builds.

## More Information

For detailed onboarding instructions, see the [SmartUI Selenium JavaScript Onboarding Guide](https://www.lambdatest.com/support/docs/smartui-onboarding-selenium-js/).
