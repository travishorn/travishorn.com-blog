---
title: "Using Grunt with Travis-CI"
datePublished: Sat Sep 13 2014 20:40:28 GMT+0000 (Coordinated Universal Time)
cuid: ckrox5uj30oolems11s0je9br
slug: using-grunt-with-travis-ci-25142062abf0
tags: javascript, web-development

---


If you are using Grunt to run your tests and Travis-CI for your continuous integration, you will need to command Travis-CI to install the grunt-cli before running your test script(s). grunt-cli is meant to be a global package, so adding it to your dependencies or devDependencies is not ideal.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627409773817/pvHsrCXe_f.png)

You’ll need the following your package.json…

* Test script command

* Grunt and any plugins as dependencies

Like so:

```
{
  "name": "projectname",
  "version": "0.0.1",
  "description": "Project description goes here.",
  "main": "index.js",
  "scripts": {
    "test": "grunt test"
  },
  "devDependencies": {
    "grunt": "~0.4.1",
    "grunt-contrib-jshint": "~0.6.0",
    "grunt-contrib-nodeunit": "~0.2.0"
  }
}
```


As an example, the following might be part of your test task in gruntfile.js:

```
module.exports = function(grunt) {
  grunt.initConfig({
    nodeunit: {
      all: ['test/**/*.js']
    },
    jshint: {
      all: ['src/**/*.js']
    }
  });

  grunt.loadNpmTasks('grunt-contrib-jshint');
  grunt.loadNpmTasks('grunt-contrib-nodeunit');
  grunt.registerTask('test', ['jshint', 'nodeunit']);
};
```


Finally, in order to run Grunt on Travis-CI, make sure your .travis.yml file looks similar to this:

```
language: node_js
node_js:
  - "0.10"
before_script:
  - npm install grunt-cli -g
```


Note the last line in particular. This will cause Travis-CI to install grunt-cli as a global package before running the grunt test task. You will need to keep that line present in your .travis.yml file, since Travis-CI [preserves no state between builds](http://about.travis-ci.org/docs/user/build-configuration/#Travis-CI-Preserves-No-State-Between-Builds). This method is [used by the Grunt project itself](https://github.com/gruntjs/grunt/blob/master/.travis.yml).

*Originally published on my old blog on September 13, 2014.*