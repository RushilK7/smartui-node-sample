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

This repository also includes examples for using SmartUI with LambdaTest Hooks integration. Hooks-based integration allows you to use SmartUI directly within your existing LambdaTest Cloud automation tests without requiring the SmartUI CLI.

### SDK vs Hooks: Which Approach to Use?

**SDK Approach (Recommended for Local Testing):**
- ✅ Works with both local and cloud execution
- ✅ Uses SmartUI CLI for configuration and execution
- ✅ Supports multiple browsers and viewports via `smartui-web.json`
- ✅ Better for CI/CD integration
- ✅ Requires `PROJECT_TOKEN` environment variable

**Hooks Approach (Recommended for Cloud-Only Testing):**
- ✅ Works only with LambdaTest Cloud Grid
- ✅ No CLI required - direct integration with LambdaTest
- ✅ Uses LambdaTest capabilities for configuration
- ✅ Better for existing LambdaTest automation suites
- ✅ Requires `LT_USERNAME` and `LT_ACCESS_KEY` environment variables

### Hooks Integration Setup

**Location:** See the `hooks` folder, where you can see all the `examples` scripts to setup your suite or run the demo.

**Purpose:** Enhance visual regression capabilities in your LambdaTest web automation tests running on LambdaTest Cloud Grid.

**Documentation:** [LambdaTest Selenium Visual Regression Documentation](https://www.lambdatest.com/support/docs/selenium-visual-regression-testing/).

### Hooks Setup Steps

#### 1. Install Dependencies

Navigate to the hooks directory and install dependencies:

```bash
cd hooks
npm i selenium-webdriver
```

#### 2. Configure Environment Variables

Set your LambdaTest credentials:

```bash
export LT_USERNAME='your_username'
export LT_ACCESS_KEY='your_access_key'
```

#### 3. Configure Capabilities

In your test file (e.g., `hooks/examples/test.js`), configure the capabilities with SmartUI options:

```javascript
const USERNAME = process.env.LT_USERNAME;
const KEY = process.env.LT_ACCESS_KEY;

let capabilities = {
  platform: "Windows 10",
  browserName: "chrome",
  version: "latest",
  visual: true,  // Enable visual testing
  "user": USERNAME,
  "accessKey": KEY,
  name: "test session",
  build: "Automation Build",
  "LT:Options": {
    "smartUI.project": "<Your Project Name>",  // Your SmartUI project name
    "smartUI.build": "<Your Build Name>",      // Optional: Build name
    "smartUI.baseline": false,                 // Set to true to update baseline
    "smartUI.options": {
      "output": {
        "errorColor": { "red": 200, "green": 0, "blue": 255 },
        "errorType": "movement",
        "transparency": 0.3,
        "largeImageThreshold": 100,
        "useCrossOrigin": false,
        "outputDiff": true
      },
      "scaleToSameSize": true,
      "ignore": "antialiasing"
    }
  }
};
```

#### 4. Connect to LambdaTest Grid

Create a WebDriver instance connected to LambdaTest Cloud:

```javascript
const GRID_HOST = "@hub.lambdatest.com/wd/hub";
const gridUrl = `https://${USERNAME}:${KEY}${GRID_HOST}`;

const driver = await new webdriver.Builder()
  .usingServer(gridUrl)
  .withCapabilities(capabilities)
  .build();
```

#### 5. Add Screenshot Hooks

Use `driver.executeScript()` to capture screenshots at any point in your test:

```javascript
// Navigate to your page
await driver.get('https://www.lambdatest.com');

// Take a screenshot with basic configuration
let config = {
  screenshotName: 'homepage-screenshot'
};
await driver.executeScript("smartui.takeScreenshot", config);

// Take a full-page screenshot
let fullPageConfig = {
  screenshotName: 'homepage-full-page',
  fullPage: true
};
await driver.executeScript("smartui.takeScreenshot", fullPageConfig);

// Take a screenshot with custom options
let customConfig = {
  screenshotName: 'homepage-custom',
  fullPage: true,
  ignore: ["antialiasing", "colors"],
  boundingBoxes: [{ x: 100, y: 100, width: 200, height: 200 }]
};
await driver.executeScript("smartui.takeScreenshot", customConfig);
```

#### 6. Run the Test

Execute your test script:

```bash
node hooks/examples/test.js
```

### Advanced Hooks Examples

#### Multiple Screenshots in One Test

```javascript
await driver.get('https://www.lambdatest.com');

// Screenshot 1: Homepage
await driver.executeScript("smartui.takeScreenshot", {
  screenshotName: 'homepage'
});

// Navigate and take another screenshot
await driver.get('https://www.lambdatest.com/pricing');
await driver.executeScript("smartui.takeScreenshot", {
  screenshotName: 'pricing-page'
});
```

#### Screenshot with Ignore Areas

```javascript
let config = {
  screenshotName: 'homepage-ignored',
  ignore: ["antialiasing"],
  ignoredBoxes: [
    { x: 0, y: 0, width: 100, height: 50 }  // Ignore header area
  ]
};
await driver.executeScript("smartui.takeScreenshot", config);
```

#### Screenshot with Bounding Boxes (Compare Specific Area)

```javascript
let config = {
  screenshotName: 'homepage-bounded',
  boundingBoxes: [
    { x: 100, y: 200, width: 800, height: 400 }  // Compare only this area
  ]
};
await driver.executeScript("smartui.takeScreenshot", config);
```

### Hooks Configuration Options

The `smartUI.options` in capabilities supports various configuration options:

- **errorColor**: RGB values for highlighting differences
- **errorType**: Type of error detection (`"movement"`, `"flat"`, `"flatDifferenceIntensity"`, `"movementDifferenceIntensity"`, `"diffOnly"`)
- **transparency**: Opacity of the error overlay (0.0 to 1.0)
- **largeImageThreshold**: Threshold for large image comparison
- **useCrossOrigin**: Enable cross-origin resource sharing
- **outputDiff**: Generate difference images
- **scaleToSameSize**: Scale images to same size before comparison
- **ignore**: Ignore specific visual artifacts (`"antialiasing"`, `"colors"`, `"less"`, `"alpha"`, `"nothing"`)

### View Hooks Results

After running your hooks-based tests, visit the [LambdaTest Automation Dashboard](https://automation.lambdatest.com/) to view:
- Test execution status
- Screenshots captured
- Visual comparison results
- Build and session details

Navigate to your SmartUI project in the [SmartUI Dashboard](https://smartui.lambdatest.com/) to see detailed visual regression results.

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

## Best Practices

### Screenshot Naming

- Use descriptive, unique names for each screenshot
- Include page/component name and state (e.g., `homepage-logged-in`, `checkout-step-2`)
- Avoid special characters that might cause issues in URLs
- Use consistent naming conventions across your test suite

### When to Take Screenshots

- **Critical user flows**: Login, checkout, payment
- **Dynamic content**: After user interactions, form submissions
- **Responsive breakpoints**: Different viewport sizes
- **State changes**: Before and after important actions

### Performance Optimization

- Take screenshots only when necessary (not on every page load)
- Use `waitForPageRender` in `smartui-web.json` for slow-loading pages
- Consider viewport-specific screenshots instead of full-page when possible
- Use ignore options to reduce false positives from dynamic content

### Error Handling

```javascript
try {
  await driver.get('https://www.lambdatest.com');
  await smartuiSnapshot(driver, "homepage");
} catch (error) {
  console.error('Screenshot failed:', error);
  // Handle error appropriately
  throw error;
} finally {
  await driver.quit();
}
```

## Common Use Cases

### Multiple Screenshots in Sequence

```javascript
await driver.get('https://www.lambdatest.com');
await smartuiSnapshot(driver, "homepage");

await driver.findElement(By.linkText('Pricing')).click();
await smartuiSnapshot(driver, "pricing-page");

await driver.findElement(By.linkText('Documentation')).click();
await smartuiSnapshot(driver, "documentation-page");
```

### Screenshot After User Interaction

```javascript
await driver.get('https://www.lambdatest.com');

// Wait for element and interact
const searchBox = await driver.findElement(By.id('search'));
await searchBox.sendKeys('Selenium');
await searchBox.sendKeys(Key.RETURN);

// Wait for results to load
await driver.wait(until.elementLocated(By.className('results')), 10000);

// Take screenshot after interaction
await smartuiSnapshot(driver, "search-results");
```

### Conditional Screenshots

```javascript
const isLoggedIn = await driver.findElements(By.className('user-menu')).length > 0;

if (isLoggedIn) {
  await smartuiSnapshot(driver, "homepage-logged-in");
} else {
  await smartuiSnapshot(driver, "homepage-guest");
}
```

## CI/CD Integration

### GitHub Actions Example

```yaml
name: SmartUI Visual Tests

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  visual-tests:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      
      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '18'
      
      - name: Install dependencies
        run: |
          cd sdk
          npm ci
      
      - name: Run SmartUI tests
        env:
          PROJECT_TOKEN: ${{ secrets.SMARTUI_PROJECT_TOKEN }}
          LT_USERNAME: ${{ secrets.LT_USERNAME }}
          LT_ACCESS_KEY: ${{ secrets.LT_ACCESS_KEY }}
        run: |
cd sdk
          npx smartui exec node sdkCloud.js
```

### Jenkins Pipeline Example

```groovy
pipeline {
    agent any
    
    environment {
        PROJECT_TOKEN = credentials('smartui-project-token')
        LT_USERNAME = credentials('lambdatest-username')
        LT_ACCESS_KEY = credentials('lambdatest-access-key')
    }
    
    stages {
        stage('Install Dependencies') {
            steps {
                sh 'cd sdk && npm ci'
            }
        }
        
        stage('Run Visual Tests') {
            steps {
                sh 'cd sdk && npx smartui exec node sdkCloud.js'
            }
        }
    }
}
```

## Troubleshooting

### Issue: `Error: SmartUI Config already exists: smartui-web.json`

**Solution**: This is informational. The config file already exists. You can:
- Use the existing config file
- Delete it and create a new one: `rm smartui-web.json && npx smartui config:create smartui-web.json`
- Use a different config file: `npx smartui --config custom-config.json exec ...`

### Issue: `PROJECT_TOKEN is required`

**Solution**: Ensure the `PROJECT_TOKEN` environment variable is set:
```bash
export PROJECT_TOKEN='your_project_token'
# Verify it's set
echo $PROJECT_TOKEN
```

### Issue: `Unauthorized, either Username or AccessKey is invalid` (Cloud)

**Solution**: 
1. Verify your credentials are set correctly:
   ```bash
   echo $LT_USERNAME
   echo $LT_ACCESS_KEY
   ```
2. Check your credentials in [LambdaTest Profile Settings](https://accounts.lambdatest.com/profile)
3. Ensure there are no extra spaces or quotes in the environment variables

### Issue: `Cannot find module '@lambdatest/selenium-driver'`

**Solution**: Install dependencies:
```bash
cd sdk
npm install @lambdatest/smartui-cli @lambdatest/selenium-driver selenium-webdriver
```

### Issue: Screenshots not appearing in SmartUI Dashboard

**Solution**:
1. Verify the `PROJECT_TOKEN` matches your SmartUI project
2. Check that the build completed successfully (no errors in terminal)
3. Wait a few moments for screenshots to process
4. Verify you're looking at the correct project in the dashboard

### Issue: `ChromeDriver version mismatch` (Local)

**Solution**: 
1. Update ChromeDriver: `npm install --save-dev chromedriver`
2. Or use WebDriver Manager: `npm install --save-dev webdriver-manager`
3. Ensure Chrome browser is up to date

### Issue: Timeout errors when taking screenshots

**Solution**: 
1. Increase `waitForPageRender` in `smartui-web.json`:
   ```json
   {
     "web": {
       "waitForPageRender": 60000
     }
   }
   ```
2. Add explicit waits before taking screenshots:
   ```javascript
   await driver.wait(until.elementLocated(By.id('main-content')), 10000);
   await smartuiSnapshot(driver, "screenshot");
   ```

### Issue: False positives in visual comparisons

**Solution**:
1. Use ignore options for dynamic content (ads, timestamps, etc.)
2. Increase `waitForTimeout` in `smartui-web.json` for lazy-loaded content
3. Use viewport-specific screenshots instead of full-page when appropriate
4. Review and update baseline screenshots when intentional changes are made

## Configuration Tips

### Optimizing `smartui-web.json`

```json
{
  "web": {
    "browsers": ["chrome", "firefox"],
    "viewports": [
      [1920, 1080],
      [1366, 768],
      [360, 640]
    ],
    "waitForPageRender": 30000,
    "waitForTimeout": 2000
  }
}
```

**Tips**:
- Start with fewer browsers/viewports for faster initial testing
- Adjust `waitForPageRender` based on your page load times
- Use `waitForTimeout` for async components (lazy-loaded images, animations)

### Environment-Specific Configuration

Create different config files for different environments:

```bash
# Development
npx smartui --config smartui-dev.json exec node sdkLocal.js

# Staging
npx smartui --config smartui-staging.json exec node sdkCloud.js

# Production
npx smartui --config smartui-prod.json exec node sdkCloud.js
```

## View Results

After running the tests, visit your SmartUI project dashboard to view the captured screenshots and compare them with baseline builds.

### Understanding Results

- **Baseline Created**: First run creates baseline screenshots
- **Passed**: Screenshot matches baseline (no visual differences)
- **Failed**: Visual differences detected
- **Review Required**: Manual review needed for differences

### Managing Baselines

- Update baseline when intentional UI changes are made
- Review failed comparisons to identify false positives
- Use ignore options to exclude known dynamic content

## Additional Resources

- [SmartUI Selenium JavaScript Onboarding Guide](https://www.lambdatest.com/support/docs/smartui-onboarding-selenium-js/)
- [LambdaTest Selenium Documentation](https://www.lambdatest.com/support/docs/selenium-automation/)
- [SmartUI Dashboard](https://smartui.lambdatest.com/)
- [LambdaTest Automation Dashboard](https://automation.lambdatest.com/)
- [LambdaTest Community](https://community.lambdatest.com/)
- [LambdaTest Blog](https://www.lambdatest.com/blog/)

## Support

For additional help:
- [LambdaTest Support](https://www.lambdatest.com/support/)
- [Documentation](https://www.lambdatest.com/support/docs/)
- [24/7 Chat Support](https://www.lambdatest.com/)
