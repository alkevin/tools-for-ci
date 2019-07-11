# Tools for Continuous Integration & Quality Code
This project was generated with [Angular CLI](https://github.com/angular/angular-cli) version 8.1.0.

## Badges
[![CircleCI](https://img.shields.io/circleci/build/gh/WingsHell/tools-for-ci/master.svg?color=%234b1&label=CircleCI&logo=CircleCI&style=plastic)](https://circleci.com/gh/WingsHell/tools-for-ci)
[![Travis (.org) branch](https://img.shields.io/travis/WingsHell/tools-for-ci/master.svg?color=%234b1&label=TravisCI&logo=travis&style=plastic)](https://travis-ci.org/WingsHell/tools-for-ci)
[![npm](https://img.shields.io/npm/v/@angular/cli.svg?color=%234b1&label=npm%20package&logo=npm&style=plastic)](https://badge.fury.io/js/%40angular%2Fcli)
![David](https://img.shields.io/david/WingsHell/tools-for-ci.svg?color=%234b1&style=plastic)
---------
[![Codacy grade](https://img.shields.io/codacy/grade/c39efc40abd0469f856a4efcfc4efe95.svg?color=%234c1&label=Codacy%20Grade&logo=codacy&style=plastic)](https://www.codacy.com/app/WingsHell/tools-for-ci?utm_source=github.com&amp;utm_medium=referral&amp;utm_content=WingsHell/tools-for-ci&amp;utm_campaign=Badge_Grade)
[![Coveralls github](https://img.shields.io/coveralls/github/WingsHell/tools-for-ci.svg?color=%234b1&label=Coveralls&style=plastic)](https://coveralls.io/github/WingsHell/tools-for-ci?branch=master)
[![Code Climate maintainability](https://img.shields.io/codeclimate/maintainability/WingsHell/tools-for-ci.svg?color=%2345D298&logo=code%20climate&style=plastic)](https://codeclimate.com/github/WingsHell/tools-for-ci/maintainability)
---------
[![GitHub](https://img.shields.io/github/license/WingsHell/tools-for-ci.svg?style=plastic)](https://www.gnu.org/licenses/gpl-3.0)

-----------------

>  WARNING : ALL THOSE TOOLS ARE FREE IF YOU WORK WITH PUBLIC REPO !

-----------------

## CircleCi : [![CircleCI](https://img.shields.io/circleci/build/gh/WingsHell/tools-for-ci/master.svg?label=CircleCI&logo=CircleCI&style=plastic)](https://circleci.com/gh/WingsHell/tools-for-ci)
 
1. Go on [CircleCI](https://circleci.com)
2. Give authorization to watch your repo on github.
3. Create forlder .circleci on root project.
4. Create .circleci/config.yml with your own config.
5. Add customLauncher to karma.conf.js.
6. Add browser compatibility to e2e/protactor-ci.conf.js.
7. Push your changes to github.
8. Go on [CircleCI](https://circleci.com).
9. Press _"start building"_.

Wait for success ! :wink: 

* Configuration :

`karma.conf.js :`

    browsers: ['Chrome'],
    customLaunchers: {
      ChromeHeadlessCI: {
        base: 'ChromeHeadless',
        flags: ['--no-sandbox', '--disable-gpu']
      }
    },

-----------------

`e2e/pratactor-ci.conf.js :`

    const config = require('./protractor.conf').config;

    config.capabilities = {
      browserName: 'chrome',
      chromeOptions: {
        args: ['--headless', '--no-sandbox', '--disable-gpu']
      }
    };

    exports.config = config;

-----------------

`.circleci/config.yml :`

    version: 2
    jobs:
      # The build job
      build:
        working_directory: ~/tools-for-ci
        docker:
          - image: circleci/node:10-browsers
        steps:
          # Checkout the code from the branch into the working_directory
          - checkout
          # Log the current branch
          - run:
              name: Show current branch
              command: echo ${CIRCLE_BRANCH}
          # Restore local dependencies from cache
          - restore_cache:
              key: tools-for-ci-{{ .Branch }}-{{ checksum "package-lock.json" }}
          # Install project dependencies
          - run: npm install
          # Cache local dependencies if they don't exist
          - save_cache:
              key: tools-for-ci-{{ .Branch }}-{{ checksum "package-lock.json" }}
              paths:
                - "node_modules"
          # Lint the source code
          - run:
              name: Linting
              command: npm run lint
          # Test the source code
          - run:
              name: Testing
              command: npm run test -- --no-watch --code-coverage --no-progress --browsers=ChromeHeadlessCI
          # Test the source code
          - run:
              name: e2e
              command: npm run e2e -- --protractor-config=e2e/protractor-ci.conf.js
          # Build project withconfiguration based on current branch
          - run:
              name: Building
              command: npm run build -- --prod

    workflows:
        version: 2
        # The build workflow
        test_and_build:
            jobs:
                - build

## TravisCI : [![Travis (.org) branch](https://img.shields.io/travis/WingsHell/tools-for-ci/master.svg?label=TravisCI&logo=travis&style=plastic)](https://travis-ci.org/WingsHell/tools-for-ci)

1. Go on [TravisCI](https://travis-ci.org).
2. Give authorization to watch your repo on github.
3. Create file .travis.yml on root project.
4. Push your changes to github.
5. Go on [TravisCI](https://travis-ci.org).

Wait for success ! :wink: 

* Configuration :

`.travis.yml :`

    dist: trusty
    sudo: false

    language: node_js
    node_js:
      - "10"

    addons:
      apt:
        sources:
          - google-chrome
        packages:
          - google-chrome-stable

    cache:
      directories:
        - ./node_modules

    before_install:
      - npm update

    install:
      - npm install

    script:
      - npm run lint
      - npm run test -- --no-watch --no-progress --browsers=ChromeHeadlessCI
      - npm run e2e -- --protractor-config=e2e/protractor-ci.conf.js
      - npm run build -- --prod

## Coveralls : [![Coveralls github](https://img.shields.io/coveralls/github/WingsHell/tools-for-ci.svg?color=%234b1&label=Coveralls&style=plastic)](https://coveralls.io/github/WingsHell/tools-for-ci?branch=master)

1. Go on [Coveralls](https://coveralls.io).
2. Give authorization to watch your repo on github.
3. Install dependencie : *npm install coveralls --save-dev*.
4. run on terminal : **ng test --no-watch --code-coverage --no-progress --browsers=ChromeHeadlessCI .
5. Look at _corevage folder_ on root project and see where is _lcov.info_ for the path.
6. Add forlder _coverage_ to *.gitignore* file if it's not.
7. Add config for Coveralls in _.travis.yml_ on root project.
8. Push your changes to github.
9. Go on [Coveralls](https://coveralls.io).

Wait for result ! :wink: 

* Configuration :

`.travis.yml : for coveralls`

    ...
    script:
    ...
      - npm run test -- --no-watch --code-coverage --no-progress --browsers=ChromeHeadlessCI
      - cat ./coverage/tools-for-ci/lcov.info | ./node_modules/
    ...

## Codacy : [![Codacy grade](https://img.shields.io/codacy/grade/c39efc40abd0469f856a4efcfc4efe95.svg?color=%234c1&label=Codacy%20Grade&logo=codacy&style=plastic)](https://www.codacy.com/app/WingsHell/tools-for-ci?utm_source=github.com&amp;utm_medium=referral&amp;utm_content=WingsHell/tools-for-ci&amp;utm_campaign=Badge_Grade)

1. Go on [Codacy](https://app.codacy.com).
2. Give authorization to watch your repo on github.
3. Push your changes to github.
4. Go on [Codacy](https://app.codacy.com).

Wait for result ! :wink: 

> No config needed :ok_hand:  Just need to config your Codacy project with issues like *quotes* for TypsScript : ctrl+f => _quotes style_ : simple quotes or disable

## Code Climate : [![Code Climate maintainability](https://img.shields.io/codeclimate/maintainability/WingsHell/tools-for-ci.svg?color=%2345D298&logo=code%20climate&style=plastic)](https://codeclimate.com/github/WingsHell/tools-for-ci/maintainability)

1. Go on [Code Climate](https://codeclimate.com).
2. Give authorization to watch your repo on github.
3. Push your changes to github.
4. Go on [Code Climate](https://codeclimate.com).

Wait for result ! :wink: 

> No config needed :ok_hand:

## Customize Badges :
> [shields.io](https://shields.io/)
