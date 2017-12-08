### AngularJS unit setup using karma and jasmine 

Units in your source code are small individual testable parts of your application.

*Unit testing* is a process in which these small units of your code 
are tested to ensure their proper operation.

*AngularJs* is designed to make *unit testing* easier but it also depends on 
how you organise the code. *Unit testing* an application where concerns are
divided into small independent units is easy rather than a piece of code that does 
everything.

In this tutorial we will learn to configure our Angular application
for *unit testing*. We will also see an example on how to test *custom filters*.

This guide requires node installed, as we will be using npm to setup *karma* and *jasmine*,
if you don’t have node installed then before proceeding further, https://nodejs.org/en.


### Details about tools


#### Karma 

*Karma* is a test runner provided by the Angular team,
*Karma* will execute your tests in multiple browsers which shall 
ensure that our application is compatible in all browsers.
https://karma-runner.github.io/1.0/index.html


#### jasmine

*Jasmine* is a javascript unit testing framework and will provide us 
with utilities to test our application. 
We can use any other javascript testing  framework for our testing but 
we’ll stick to jasmine as it is the most popular.
https://jasmine.github.io/2.0/introduction.html






### Configuration

Cnage you directory to your project in command line

##### Install AngularJS 

If you have already installed the angular js then no need to install.

Otherwise, run the command `npm install angular --save`

##### Install karma 

`npm install karma --save-dev`

##### Install Jasmine 

`npm install karma-jasmine jasmine-core --save-dev`




##### Install ngMock

*ngMock* allows you to inject and mock angular services to 
help you test your application. https://docs.angularjs.org/api/ngMock

`npm install angular-mocks --save-dev`




##### Browsers

Install browser launcher on which you want karma to run your tests.
We need to install atleast one browser. I’ll use *PhantomJs*.

`npm install karma-phantomjs-launcher --save-dev`

##### Creating directory 

1. Create `unitTest` directory in your root project. and inside this directory,
1. Create`app` //your script files, controllers,filters etc.
1. Create `mkdir tests` //here we will keep our tests.


##### karma.conf.js

1. `karma init`
2. Select Jasmine as your testing framework.
3. Select browser, I’ve selected PhantomJS.
4. Specify the paths to your js and spec files. Eg. `app/*.js`, `test/*.js`.
5. After answering a few more questions you should be done.
6. Open up your `karma.conf.js` and add the location of angular.js in to the files array.
    a. `node_modules/angular/angular.js`
7. Add the location for ngMock just below that.
    a. `node_modules/angular-mocks/angular-mocks.js`


##### Karma configuration file 

```
// Karma configuration
// Generated on Sat Jul 25 2015 19:12:21 GMT+0530 (India Standard Time)
module.exports = function(config) {
  config.set({
    // base path that will be used to resolve all patterns (eg. files, exclude)
    basePath: '',
    // frameworks to use
    // available frameworks: https://npmjs.org/browse/keyword/karma-adapter
    frameworks: ['jasmine'],
    // list of files / patterns to load in the browser
    files: [
      'node_modules/angular/angular.js',
      'node_modules/angular-mocks/angular-mocks.js',
      'app/app.js',  //use wildcards in real apps
      'tests/tests.js' //use wildcards in real apps
    ],
    // list of files to exclude
    exclude: [
    ],
    // preprocess matching files before serving them to the browser
    // available preprocessors: https://npmjs.org/browse/keyword/karma-preprocessor
    preprocessors: {
    },
    // test results reporter to use
    // possible values: 'dots', 'progress'
    // available reporters: https://npmjs.org/browse/keyword/karma-reporter
    reporters: ['progress'],
    // web server port
    port: 9876,
    // enable / disable colors in the output (reporters and logs)
    colors: true,
    // level of logging
    // possible values: config.LOG_DISABLE || config.LOG_ERROR || config.LOG_WARN || config.LOG_INFO || config.LOG_DEBUG
    logLevel: config.LOG_INFO,
    // enable / disable watching file and executing tests whenever any file changes
    autoWatch: true,
    // start these browsers
    // available browser launchers: https://npmjs.org/browse/keyword/karma-launcher
    browsers: ['PhantomJS'],
    // Continuous Integration mode
    // if true, Karma captures browsers, runs the tests and exits
    singleRun: false
  })
}
```


This is the minimum configuration that you'll need to test your 
angular application with karma and jasmine.

#### Update the folowing scripts in your files.

##### app.js 

```
angular.module('MyApp', [])
.filter('reverse',[function(){
    return function(string){
        return string.split('').reverse().join('');
    }
}])
```

##### test.js 

```
describe('Filters', function(){ //describe your object type
    beforeEach(module('MyApp')); //load module
    describe('reverse',function(){ //describe your app name
        var reverse;
        beforeEach(inject(function($filter){ //initialize your filter
            reverse = $filter('reverse',{});
        }));
        it('Should reverse a string', function(){  //write tests
            expect(reverse('rahil')).toBe('lihar'); //pass
            expect(reverse('don')).toBe('nod'); //pass
            //expect(reverse('jam')).toBe('oops'); // this test should fail
        });
    });
});
```

As you see above we are first describing the object type then loading our
angular app. Next we are describing the object by name and initializing our
filter then we write our test in the it block along with expectations.

### Runn test 

`karma start`. make sure that you are in the unitTesting directory.




Improtant Note: If you are using *npm v 3.x* you might face some problems while 
running *karma start*. The reason is the way in which *npm 3.x* installs dependencies.
The solution we found was to *install phantomjs-prebuilt@2.1.4* globally and as well 
as locally. Also, *jasmine-core* is required to be installed globally. 
eg: *npm install jasmine-core -g*

If all the tests passed you should see *Executated 1 of 1 success*. 

If any error occur, then it shows *Executad 1 of 1 failed* or other message.

If you are still facing error on setup,Please watch this tutorial 
https://www.youtube.com/watch?v=f7lIBiLmISQ&feature=youtu.be


*Visite :* for controller testing 
https://ciphertrick.com/2016/06/26/unit-testing-controllers-angularjs-karma-jasmine/