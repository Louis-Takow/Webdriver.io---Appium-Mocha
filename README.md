# Mobile Automation Framework Setup

This guide provides a step-by-step setup for a mobile automation testing framework using: 
1. **WebdriverIO as a Test Runner**:
WebdriverIO acts as a test runner and provides a structured way to write, organize, and execute tests for both web and mobile applications.
2. **Appium as an Automation Engine**:
Appium serves as the underlying automation engine that communicates with mobile devices to perform actions on the app's UI.
3. **Spec Reporter**:
To give detail report after test execution.

**Together for Mobile Testing**:
When combined, WebdriverIO can leverage Appium’s capabilities to automate mobile applications effectively while providing a rich API for test writing and integration with other tools.We’ll configure an Android emulator to run the tests on a specified APK file.

## Prerequisites

1. **Node.js**: Download and install [Node.js](https://nodejs.org/en/).
2. **Java Development Kit (JDK)**: Download and install the JDK. Ensure the JAVA_HOME environment variable is set.
3. **Android Studio**: Download and install [Android Studio](https://developer.android.com/studio), which includes Android SDK tools. Set android SDK path as ANDROID_HOME in enviroment vairable.
4. **VS Code**: Download and install VS Code IDE
5. **Appium Desktop**: Download and install Appium desktop
6. **Appium**: Install Appium globally by running on terminal:
   ```bash
   npm install -g appium
   ```
6. **Appium Doctor**: Install Appium doctor globally by running on terminal:
   ```bash
   npm install -g appium-doctor
   ```
## Step 1: Initialize the Project

1. Create and navigate to a new project folder:
   ```bash
   mkdir mobile-automation-framework
   cd mobile-automation-framework
   ```
2. **Clone this repository** through terminal of new project folder.

   ```bash
   git clone <repository-url>
   cd Webdriver.io---Appium-Mocha
   ```
3. Initialize a Node.js project:
   ```bash
   npm init -y
   ```

## Step 2: Install WebdriverIO and Related Packages

Install WebdriverIO CLI, Appium service, Mocha framework, and Spec reporter:

```bash
npm install @wdio/cli @wdio/appium-service @wdio/mocha-framework @wdio/spec-reporter
```

## Step 3: Configure WebdriverIO

Run the WebdriverIO configuration wizard:

```bash
npx wdio config
```

During configuration:
- **A project named "demo" was detected at "C:\Users\joelt\Desktop\Demo", correct**? Select `yes`
- **What type of testing would you like to do?** Select `E2E Testing - of Web or Mobile Applications`
- **Where is your automation backend located?** Select `my local machine`
- **Which environment you would like to automate?** Select `Mobile - native, hybrid and mobile web apps, on Android or iOS`
- **Which mobile environment you'ld like to automate?** Select `Android- native, hybrid and mobile web apps, tested on emulators and real devices using UiAutomator2 (https://www.npmjs.com/package/appium-uiautomator2-driver)`
- **Which framework do you want to use?** Select `Mocha` 
- **Do you want to use Typescript to write tests?** Select `no`
- **Do you want WebdriverIO to autogenerate some test files?** Select `yes`
- **What should be the location of your spec files?** Select `C:\Users\joelt\Desktop\Demo\test\specs\**\*.js`
- **Which reporter do you want to use?** Select `Spec()`
- **Do you want to add a service to your test setup?** Select `Appium`
- **Do you want me to run npm install** Select `yes`
- **Continue with Appium setup using appium-installer (https://github.com/AppiumTestDistribution/appium-installer)?** Select `yes`
- **Select Options** `Run Appium Doctor` and `Need help setting up Android Environment to run your Appium test?`


### Example `wdio.conf.js` Configuration

Below is a sample configuration to use WebdriverIO with Appium and Mocha:

```javascript
exports.config = {
    runner: 'local',
    specs: [
        './test/specs/**/*.js'
    ],
    exclude: [],
    maxInstances: 1,
    capabilities: [{
        platformName: 'Android',
        'appium:deviceName': 'emulator-5554',
        'appium:platformVersion': '11.0',
        'appium:app': 'C:\\Users\\joelt\\Downloads\\Nkwa.apk',  // Path to your APK file
        'appium:automationName': 'UiAutomator2',
    }],
    services: ['appium'],
    framework: 'mocha',
    reporters: ['spec'],
    mochaOpts: {
        timeout: 60000
    }
};
```

### Important Configuration Notes
- To get attached `deviceName` enter command on terminal:
   ```bash
   adb shell getprop ro.product.model
   ```
- To get `platformVersion` enter command on terminal:
   ```bash
   adb shell getprop ro.build.version.release
   ```
- To get `appPackageName` and `appActivity`enter command on terminal:
   ```bash
   adb shell dumpsys window | findstr /R "mCurrentFocus"
   ```
- Set the `appium:app` capability to the full path of your APK file (e.g., `C:\\Users\\Username\\Downloads\\ApkName.apk`).

## Step 4: Set Up an Android Emulator

1. Open **Android Studio** and navigate to **AVD Manager** from **Tools > Device Manager**.
2. Create a new virtual device:
   - **Phone**: Select a device (e.g., Pixel 4).
   - **System Image**: Choose an image that matches the platform version in your `wdio.conf.js` (e.g., Android 11).
3. Launch the emulator:
   - Once created, launch the emulator from AVD Manager.

## Step 5: Run Appium Server
- Open `Appium desktop`
- Start server will default port number

Appium will listen by default on `http://127.0.0.1:4723`.

## Step 6: Inspect Element locators with Appium Inspector
- Start `inspector session`
- Setup `desired capabilities`and start session

## Step 7: Create/View a Test Script

- Open project directory with VS Code IDE
- In the `test/specs` folder, create/view test files (e.g., `register.e2e.js`)
- Test script are written using `mocha framework`

## Step 8: Run Tests
1. ### Login Tests
To execute the login tests, use the following command:

```bash
npx wdio run wdio.conf.js --spec test/specs/login.e2e.js  
```
2. ### Register Test
To execute the register test, use the following command:

```bash
npx wdio run wdio.conf.js --spec test/specs/register.e2e.js
```
## Troubleshooting

- **Emulator Connection**: Ensure the emulator is running before starting the test. Use `adb devices` to verify the device is connected.
- **APK Path**: Verify the APK file path is correct in the `wdio.conf.js` file.
- **Appium Service**: Ensure Appium is running on the default port (4723) or update the configuration to match your setup.

## Additional Notes

- To inspect elements on your app, use **Appium Inspector**

---

This setup provides the foundation for mobile automation testing using WebdriverIO, Appium, and Mocha on an Android emulator. Modify the sample test case and configuration as needed for your specific project requirements.
