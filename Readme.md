# Introduction

Write a plugin in openFrameworks. Write an app which can load those plugins at runtime in openFrameworks. You have:

* Application has `main.cpp`
* Plugin has `plugin.cpp`

Example pattern:
* Application contains an abstract class definition `BaseShape`
* Plugin has a class `CircleShape` which inherits `BaseShape`
* Application loads Plugin, and now can create instances of `CircleShape`

## Naming conventions:
* `Module` - A (non-abstract) class which the Plugin provides (this is the content of your plugin, your plugin may define 1 or more `Module`s)
* `ModuleBaseType` - A (abstract) class which the Module inherits from
* `Factory<Module>` - A class which can instantiate other classes, e.g. a `Factory<MyModule>` can create instances of `MyModule` class
* `FactoryRegister<ModuleBaseType>` - A class which contains a list of `Factory`s. All the `Factory`s inside this register will create `Module`s which inherit from `ModuleBaseType`.

# Requirements / Compatability

Currently runs on oF 0.9.0 in Windows. Thanks to work by @satoruhiga, ofxPlugin will compile in OSX without issues, but the main plugin loading features are still untested.

Tested on Visual Studio 2015, but should work on earlier versions too (be aware of changing the `Platform Toolset` option in all the relevant projects).

# Notes

## Usage pattern

A `Factory` is something which instantiates classes for you. For each module (i.e. class type), you'll have one `Factory` which can instantiate that class at runtime.

ofxPlugin provides a `FactoryRegister` where you can store all these factories. When you load a plugin, it adds the factories from that plugin to the `FactoryRegister`

Your `ModuleBaseType` must be a non abstract class, i.e. it cannot have something like `virtual void draw() = 0;`. This hurts I know, but it's necessary for a factory model to work in many situations.

Your plugin needs references to openframeworksLib, ofxSingletonLib, and anything else it references.

# Current limitations / Known issues

* All plugins in one dll must inherit from the same `ModuleBaseType`
* Plugins are never unloaded until the application terminates (you can still manually remove factories from the `FactoryRegister`).
