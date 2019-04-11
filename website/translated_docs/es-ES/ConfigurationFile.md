---
id: configurationfile
title: Configuración de Testrunner
---
El archivo de configuración contiene toda la información necesaria para ejecutar su suite de pruebas. Es un módulo de Node que exporta un JSON. Aquí hay una configuración de ejemplo con todas las propiedades soportadas e información adicional:

```js
exports.config = {

    // =====================
    // Configuraciones del servidor
    // =====================
    // Dirección del servidor que corre Selenium. Esta información usualmente no se necesita 
    // ya que WebdriverIO se conecta automaticamente al localhost. También si usas uno de
    / soportan servicios de nube como salsa Labs, Browserstack o Bot prueba también no
    / / necesario para definir información de host y puerto porque puede WebdriverIO figura que fuera
    / / según la información de su usuario y clave. Sin embargo, si está usando un Selenium privado
    // backend debe definir la dirección de host, puerto y ruta aquí.
    //
    host: '0.0.0.0',
    port: 4444,
    path: '/wd/hub',
    //
    // =================
    // Proveedores de Servicios
    // =================
    // WebdriverIO soporta Sauce Labs, Browserstack y Testing Bot (otros proveedores Cloud
    // también deberían funcionar). Estos servicios definen usuario y clave específicos (o clave de acceso)
    // Cuyos valores se necesitan especificar aquí para conectarse a estos servicios.
    //
    user: 'webdriverio',
    key:  'xxxxxxxxxxxxxxxx-xxxxxx-xxxxx-xxxxxxxxx',
    //
    // Si ejecutas tus pruebas en SauceLabs puedes especificar la región que quieres ejecutar tus pruebas
    // en la propiedad `region`. Las abreviaturas para regiones disponibles son:
    // us: us-west-1 (default)
    // eu: eu-central-1
    region: 'us',
    //
    // ==================
    // Especifica los Archivos de Pruebas
    // ==================
    // Define cuales specs de pruebas deben ejecutarse. El patrón es relativo al directorio
    // desde donde se llamó a `wdio`. Notese que, si está llamando `wdio` desde un script
    // NPM (vea https://docs.npmjs.com/cli/run-script) entonces el actual directorio
    // de trabajo es donde reside su package.json, así que `wdio` será llamado desde allí.
    //
    specs: [
        'test/spec/**'
    ],
    // Patrones a excluir.
    exclude: [
        'test/spec/multibrowser/**',
        'test/spec/mobile/**'
    ],
    //
    // ============
    // Capacidades
    // ============
    // Defina las capacidades aqui. WebdriverIO puede ejecutar múltiples capacidades al mismo
    // tiempo. Depending on the number of capabilities, WebdriverIO launches several test
    // sessions. Within your capabilities you can overwrite the spec and exclude option in
    // order to group specific specs to a specific capability.
    //
    //
    // First you can define how many instances should be started at the same time. Let's
    // say you have 3 different capabilities (Chrome, Firefox and Safari) and you have
    // set maxInstances to 1, wdio will spawn 3 processes. Therefor if you have 10 spec
    // files and you set maxInstances to 10, all spec files will get tested at the same time
    // and 30 processes will get spawned. The property basically handles how many capabilities
    // from the same test should run tests.
    //
    maxInstances: 10,
    //
    // Or set a limit to run tests with a specific capability.
    maxInstancesPerCapability: 10,
    //
    // If you have trouble getting all important capabilities together, check out the
    // Sauce Labs platform configurator - a great tool to configure your capabilities:
    // https://docs.saucelabs.com/reference/platforms-configurator
    //
    capabilities: [{
        browserName: 'chrome',
        chromeOptions: {
        // to run chrome headless the following flags are required
        // (see https://developers.google.com/web/updates/2017/04/headless-chrome)
        // args: ['--headless', '--disable-gpu'],
        }
    }, {
        // maxInstances can get overwritten per capability. So if you have an in house Selenium
        // grid with only 5 firefox instance available you can make sure that not more than
        // 5 instance gets started at a time.
        maxInstances: 5,
        browserName: 'firefox',
        specs: [
            'test/ffOnly/*'
        ],
        "moz:firefoxOptions": {
          // flag to activate Firefox headless mode (see https://github.com/mozilla/geckodriver/blob/master/README.md#firefox-capabilities for more details about moz:firefoxOptions)
          // args: ['-headless']
        }
    }],
    //
    // Additional list of node arguments to use when starting child processes
    execArgv: [],
    //
    // ===================
    // Test Configurations
    // ===================
    // Define all options that are relevant for the WebdriverIO instance here
    //
    // Level of logging verbosity: trace | debug | info | warn | error
    logLevel: 'info',
    //
    // If you only want to run your tests until a specific amount of tests have failed use
    // bail (default is 0 - don't bail, run all tests).
    bail: 0,
    //
    // Set a base URL in order to shorten url command calls. If your `url` parameter starts
    // with `/`, the base url gets prepended, not including the path portion of your baseUrl.
    // If your `url` parameter starts without a scheme or `/` (like `some/path`), the base url
    // gets prepended directly.
    baseUrl: 'http://localhost:8080',
    //
    // Default timeout for all waitForXXX commands.
    waitforTimeout: 1000,
    //
    // Add files to watch (e.g. application code or page objects) when running `wdio` command
    // with `--watch` flag (globbing is supported).
    filesToWatch: [
        // e.g. rerun tests if I change my application code
        // './app/**/*.js'
    ],
    //
    // Framework you want to run your specs with.
    // The following are supported: mocha, jasmine and cucumber
    // see also: http://webdriver.io/docs/frameworks.html
    //
    // Make sure you have the wdio adapter package for the specific framework installed before running any tests.
    framework: 'mocha',
    //
    // Test reporter for stdout.
    // The only one supported by default is 'dot'
    // see also: http://webdriver.io/docs/dot-reporter.html and click on "Reporters" in left column
    reporters: [
        'dot',
        ['allure', {
            //
            // If you are using the "allure" reporter you should define the directory where
            // WebdriverIO should save all allure reports.
            outputDir: './'
        }]
    ],
    //
    // Options to be passed to Mocha.
    // See the full list at http://mochajs.org/
    mochaOpts: {
        ui: 'bdd'
    },
    //
    // Options to be passed to Jasmine.
    // See also: https://github.com/webdriverio/webdriverio/tree/master/packages/wdio-jasmine-framework#jasminenodeopts-options
    jasmineNodeOpts: {
        //
        // Jasmine default timeout
        defaultTimeoutInterval: 5000,
        //
        // The Jasmine framework allows it to intercept each assertion in order to log the state of the application
        // or website depending on the result. For example it is pretty handy to take a screenshot every time
        // an assertion fails.
        expectationResultHandler: function(passed, assertion) {
            // do something
        },
        //
        // Make use of Jasmine-specific grep functionality
        grep: null,
        invertGrep: null
    },
    //
    // If you are using Cucumber you need to specify where your step definitions are located.
    // See also: https://github.com/webdriverio/webdriverio/tree/master/packages/wdio-cucumber-framework#cucumberopts-options
    cucumberOpts: {
        require: [],        // <string[]> (file/dir) require files before executing features
        backtrace: false,   // <boolean> show full backtrace for errors
        compiler: [],       // <string[]> ("extension:module") require files with the given EXTENSION after requiring MODULE (repeatable)
        dryRun: false,      // <boolean> invoke formatters without executing steps
        failFast: false,    // <boolean> abort the run on first failure
        format: ['pretty'], // <string[]> (type[:path]) specify the output format, optionally supply PATH to redirect formatter output (repeatable)
        colors: true,       // <boolean> disable colors in formatter output
        snippets: true,     // <boolean> hide step definition snippets for pending steps
        source: true,       // <boolean> hide source URIs
        profile: [],        // <string[]> (name) specify the profile to use
        strict: false,      // <boolean> fail if there are any undefined or pending steps
        tags: [],           // <string[]> (expression) only execute the features or scenarios with tags matching the expression
        timeout: 20000,      // <number> timeout for step definitions
        ignoreUndefinedDefinitions: false, // <boolean> Enable this config to treat undefined definitions as warnings.
    },
    //
    // =====
    // Hooks
    // =====
    // WebdriverIO provides a several hooks you can use to interfere the test process in order to enhance
    // it and build services around it. You can either apply a single function to it or an array of
    // methods. If one of them returns with a promise, WebdriverIO will wait until that promise got
    // resolved to continue.
    //

    /**
     * Gets executed once before all workers get launched.
     * @param {Object} config wdio configuration object
     * @param {Array.<Object>} capabilities list of capabilities details
     */
    onPrepare: function (config, capabilities) {
    },
    /**
     * Gets executed just before initialising the webdriver session and test framework. It allows you
     * to manipulate configurations depending on the capability or spec.
     * @param {Object} config wdio configuration object
     * @param {Array.<Object>} capabilities list of capabilities details
     * @param {Array.<String>} specs List of spec file paths that are to be run
     */
    beforeSession: function (config, capabilities, specs) {
    },
    /**
     * Gets executed before test execution begins. At this point you can access to all global
     * variables like `browser`. It is the perfect place to define custom commands.
     * @param {Array.<Object>} capabilities list of capabilities details
     * @param {Array.<String>} specs List of spec file paths that are to be run
     */
    before: function (capabilities, specs) {
    },
    /**
     * Hook that gets executed before the suite starts
     * @param {Object} suite suite details
     */
    beforeSuite: function (suite) {
    },
    /**
     * Hook that gets executed _before_ a hook within the suite starts (e.g. runs before calling
     * beforeEach in Mocha)
     */
    beforeHook: function () {
    },
    /**
     * Hook that gets executed _after_ a hook within the suite ends (e.g. runs after calling
     * afterEach in Mocha)
     */
    afterHook: function () {
    },
    /**
     * Function to be executed before a test (in Mocha/Jasmine) or a step (in Cucumber) starts.
     * @param {Object} test test details
     */
    beforeTest: function (test) {
    },
    /**
     * Runs before a WebdriverIO command gets executed.
     * @param {String} commandName hook command name
     * @param {Array} args arguments that command would receive
     */
    beforeCommand: function (commandName, args) {
    },
    /**
     * Runs after a WebdriverIO command gets executed
     * @param {String} commandName hook command name
     * @param {Array} args arguments that command would receive
     * @param {Number} result 0 - command success, 1 - command error
     * @param {Object} error error object if any
     */
    afterCommand: function (commandName, args, result, error) {
    },
    /**
     * Function to be executed after a test (in Mocha/Jasmine) or a step (in Cucumber) ends.
     * @param {Object} test test details
     */
    afterTest: function (test) {
    },
    /**
     * Hook that gets executed after the suite has ended
     * @param {Object} suite suite details
     */
    afterSuite: function (suite) {
    },
    /**
     * Gets executed after all tests are done. You still have access to all global variables from
     * the test.
     * @param {Number} result 0 - test pass, 1 - test fail
     * @param {Array.<Object>} capabilities list of capabilities details
     * @param {Array.<String>} specs List of spec file paths that ran
     */
    after: function (result, capabilities, specs) {
    },
    /**
     * Gets executed right after terminating the webdriver session.
     * @param {Object} config wdio configuration object
     * @param {Array.<Object>} capabilities list of capabilities details
     * @param {Array.<String>} specs List of spec file paths that ran
     */
    afterSession: function (config, capabilities, specs) {
    },
    /**
     * Gets executed after all workers got shut down and the process is about to exit.
     * @param {Object} exitCode 0 - success, 1 - fail
     * @param {Object} config wdio configuration object
     * @param {Array.<Object>} capabilities list of capabilities details
     * @param {<Object>} results object containing test results
     */
    onComplete: function (exitCode, config, capabilities, results) {
    },
    /**
    * Gets executed when an error happens, good place to take a screenshot
    * @ {String} error message
    */
    onError: function(message) {
    }
    /**
     * Cucumber specific hooks
     */
    beforeFeature: function (feature) {
    },
    beforeScenario: function (scenario) {
    },
    beforeStep: function (step) {
    },
    afterStep: function (stepResult) {
    },
    afterScenario: function (scenario) {
    },
    afterFeature: function (feature) {
    }
};
```

You can also find that file with all possible options and variations in the [example folder](https://github.com/webdriverio/webdriverio/blob/master/examples/wdio.conf.js).