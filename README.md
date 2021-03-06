

cordova-plugin-config-command plugin [![Build Status](https://travis-ci.org/dpa99c/cordova-plugin-config-command.png)](https://travis-ci.org/dpa99c/cordova-plugin-config-command) [![Latest Stable Version](https://img.shields.io/npm/v/cordova-plugin-config-command.svg)](https://www.npmjs.com/package/cordova-plugin-config-command) [![Total Downloads](https://img.shields.io/npm/dt/cordova-plugin-config-command.svg)](https://npm-stat.com/charts.html?package=cordova-plugin-config-command)
============================


<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**

- [Overview](#overview)
  - [Why should I use it?](#why-should-i-use-it)
  - [Important note for PhoneGap Build / Intel XDK](#important-note-for-phonegap-build--intel-xdk)
- [Installation](#installation)
- [Usage](#usage)
  - [Attributes](#attributes)
  - [Example usage](#example-usage)
- [License](#license)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# Overview

This Cordova/Phonegap plugin enables CLI commands to be defined in a project's `config.xml` and be executed at the various [hook points](https://cordova.apache.org/docs/en/latest/guide/appdev/hooks/index.html) when running Cordova/Phonegap CLI commands.

<!-- DONATE -->
[![donate](https://www.paypalobjects.com/en_US/i/btn/btn_donateCC_LG_global.gif)](https://www.paypal.com/cgi-bin/webscr?cmd=_s-xclick&hosted_button_id=ZRD3W47HQ3EMJ)

I dedicate a considerable amount of my free time to developing and maintaining this tool, along with my other Open Source software.
To help ensure this tool is kept updated, new features are added and bugfixes are implemented quickly, please donate a couple of dollars (or a little more if you can stretch) as this will help me to afford to dedicate time to its maintenance. Please consider donating if you're using this tool in an app that makes you money, if you're being paid to make the app, if you're asking for new features or priority bug fixes.
<!-- END DONATE -->


## Why should I use it?

- If you want to run custom commands during the Cordova build process or on other Cordova CLI commands.
- For example, if you want to run a custom test suite or validator tool.
- While you can achieve the same effect by creating your own hook scripts or wrapping the Cordova CLI commands with another tool such as Grunt or Gulp, the intention of this plugin is to provide convenience.


## Important note for PhoneGap Build / Intel XDK

- This plugin **WILL NOT WORK** with [Phonegap Build](https://build.phonegap.com/) because it relies on using [hook scripts](https://cordova.apache.org/docs/en/latest/guide/appdev/hooks/) which are [not supported by Phonegap Build](https://github.com/phonegap/build/issues/279).
- The same goes for [Intel XDK](https://software.intel.com/en-us/intel-xdk) which also [does not support hook scripts](https://software.intel.com/en-us/xdk/docs/add-manage-project-plugins).
- If you are using another cloud-based Cordova/Phonegap build service and find this plugin doesn't work, the reason is probably also the same.

# Installation

The plugin is registered on [npm](https://www.npmjs.com/package/cordova-plugin-config-command) as `cordova-plugin-config-command`

To install the plugin using the CLI:

    $ cordova plugin add cordova-plugin-config-command --save
    $ phonegap plugin add cordova-plugin-config-command --save

# Usage

- The plugin enables you to define `<command>` elements in the `config.xml` which the plugin translates into commands which are executed on a given Cordova hook.
- The `<command>` elements can either be placed at the top-level (within `<widget>`) to run across all platforms, or inside `<platform>` elements to run only for a specific platform operation.
- One or more `<command>` elements may be placed in the `config.xml`. They will be executed in the order they appear in the XML document.


## Attributes

- `name` **(required)** - name (and optionally relative/absolute path) of the command to execute.
    - Arguments for the command may optionally be defined here or using the `args` attribute.
- `hook` **(required)** - Cordova hook on which to execute the command. For a list of available hooks see the [Cordova Hooks guide](https://cordova.apache.org/docs/en/latest/guide/appdev/hooks/index.html).
- `args` (optional) - Arguments for the command may optionally be defined here or within the `name` attribute.
    - Argument values must be XML-escaped: for example double quotes must be entered as `&quot;` and ampersands as `&amp;`.
- `display_output` (optional) - If set to `true`, the console output from the command will be displayed. Defaults to false.
- `abort_on_error` (optional) - If set to `true`, and the command results in a non-zero error code, the Cordova operation will be aborted. Defaults to false.
- `cwd` (optional) - The directory from which to execute the command. Defaults to the root of the Cordova project.

## Example usage

    <command name="echo" args="&quot;My grammar's getting worse &amp; worse&quot"
        hook="before_prepare" display_output="true"/>

    <!-- Run npm test script -->
    <command name="npm" args="test" hook="before_prepare" display_output="true" abort_on_error="true"/>

    <!-- Validate with JSHint -->
    <command name="jshint" args="www" hook="after_prepare" display_output="true" abort_on_error="true"/>

    <!-- Run Jasmine tests -->
    <command name="jasmine" args="spec/*Spec.js" hook="before_prepare" display_output="true" abort_on_error="true"/>

    <platform name="android">
        <!-- Run Maven tests -->
        <command name="mvn" args="surefire:test" hook="after_build" display_output="true" abort_on_error="true"/>
    </platform>

    <platform name="ios">
        <!-- Run Xcode tests -->
        <command
            name="xcodebuild"
            args="test -project platforms/ios/MyApp.xcodeproj -scheme MyApp -destination 'platform=OS X,arch=x86_64'"
            hook="after_build"
            display_output="true"
            abort_on_error="true"
         />

        <!-- Validate build -->
        <command
            name="altool"
            cwd="/Applications/Xcode.app/Contents/Applications/Application\ Loader.app/Contents/Frameworks/ITunesSoftwareService.framework/Support/"
            args="--validate-app --file platforms/ios/build/device/my-app.ipa --username itunesconnect@user.com --password password"
            hook="after_build"
            display_output="true"
            abort_on_error="true"
        />
    </platform>

# License
================

The MIT License

Copyright (c) 2016 Dave Alden / Working Edge Ltd.

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in
all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
THE SOFTWARE.
