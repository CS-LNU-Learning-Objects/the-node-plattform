# NPM - The Package manager for Node.js
Most of modern programming languages have some kind of a package manager, a way to share and use code. For JavaScript/Node that is NPM, a package manager for node modules. NPM is bundle with node and should be installed when you install node.js. NPM is the tool that help us to install, organize and use node packages. That could be modules developed by you or by other developers and that is available through the [NPM web site (https://www.npmjs.com/)](https://www.npmjs.com/). This could be used as a global service where developers contribute with modules  you can use in your applications. Remember, small core, many and small modules/packages.

This text will focus on the use of NPM when you develop new node applications and not how you publish your own modules. This is not a complete guide so be prepared to seek information in the NPM-documentation.

## Understand the NPM cli
When you got node.js installed you also got the NPM program which let you write NPM commands. Open up your terminal window and write:

```
npm -v
```
This will output the version of NPM that is installed on your system. The NPM program have a lots of command we need to know about to be effective in our work.

### Install NPM modules
The most common task is to install a package to use in your program. A very common module is [lodash (https://www.npmjs.com/package/lodash)](https://www.npmjs.com/package/lodash) which is a utility library for javascript. Lets say we want to use that package in our program. Be sure that you are standing in your project folder in your terminal program. Then write:

```
npm install lodash
```
That means that you are going to install the lodash package in your project folder. You may see that a new folder is created, *node_modules*. This is the folder node will look for installed modules when you trying to require/use them in your own code. If you look in that folder you will find the module and all its code installed in the lodash folder.

We have installed the module locally, that is installed it in our project folder. This is the most common way to work with modules in your applications.

Some times a module could be installed globally, installed so we can use it from anywhere in our system. If we want to install the module globally you add the -g flag when installing:
```
npm install <name of the module> -g
```
The NPM program will then install the module in its global folder which depends on the operating system you´re using and in what folder node is installed. [More info about this at https://docs.npmjs.com/files/folders.](https://docs.npmjs.com/files/folders)

## The package.json file
Every node project should have a package.json file (at least) in its root folder. This is a meta file that describes your project. Things like author, version, repo-URL is written to this file. The package.json also describes what dependencies your project has, that is what other NPM packages it depends on.

Lets say you push your project to a GitHub repository for others to download and run. Should you also push up all the modules you using? It could be hundreds of folders with code. You probably put the node_modules-folder in your .gitignore file to avoid that and let the users install the modules by them self by reading in the package.json which modules your project needs. By running the command:
```
npm install
```
node will look for the package.json file and figure out which modules it will need to install.

So when you install a module in your project with "npm install <name of the module>" you should also add the module name to your package.json. You can do this by using the flag --save or --save-dev, where --save-dev is for packages we´re just using when developing and not in production. That could be packages for testing, hinting, code coverage and things like that. [Since version > 5.0 of npm the `--save` flag will be added by default](http://blog.npmjs.org/post/161081169345/v500). An example of this could be:
```
npm install lodash --save
```
and
```
npm install mocha --save-dev
```

If you run these two command your package.json file would look something like:

```
{
  "name": "my-project",
  "version": "1.0.0",
  "description": "This is a test ",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [
    "test",
    "removeme"
  ],
  "author": "John Häggerud",
  "license": "ISC",
  "dependencies": {
    "lodash": "^3.10.1"
  },
  "devDependencies": {
    "mocha": "^2.3.4"
  }
}
```
As you see this is pure JSON and you can probably figure out most of the stuff. You can also see that we have two keys "devDependencies" and "dependencies" where the installed packages is listed. Also observe that each package is followed by the version number that should be used.

As you can see it also specifies which version (pr major/minor version) to install when running "npm install" on this package-file.
There are many ways of specifying versions in the package.json. More information see: https://docs.npmjs.com/files/package.json#dependencies

If no sign is provided it will always install just that version. There are different opinions about how to do this. The default way is using ^-character in front of the version number to allow bug fixes and backward compatibility but some people advise to just use the specified version number (without any character in front) to avoid version problem when going to production. The version system used by node-modules is built on the [Semantic Versioning guidelines.](http://semver.org/)

## package-lock.json
As you may wonder this could lead to problems, especially when handling modules in a production environment. 
By default, ´npm install´  recursively installs all the dependencies that the modules specified in your package.json 
have using the above described semver patterns. When shipping code we don´t want some modules be of an other versions 
then the one used when building the software. 
We want guarantee that a developer of a module in our dependency chain won´t be able to make a update that potentially 
can break or build. If we only put our trust in package.json this could happened.

Therefor will npm also create the package-lock.json-file. This file holds the single representation of the dependency tree so that
other developers (or build scripts) will install the exactly same versions of the dependencies. For more information please read: https://docs.npmjs.com/files/package-locks

## NPM Shrinkwrap
To fix this problem we can use the `npm shrinkwrap` command. For more information [consult the documentation](https://docs.npmjs.com/cli/shrinkwrap).


## NPM as a build tool
Most front-end developers know about build tools as [Grunt](http://gruntjs.com/) or [Gulp](http://gulpjs.com/). But you can also use npm for this.
This is out of scope for this text but if you are interested in this you should check out [Kate Hudsons talk from nordic.js 2015](https://www.youtube.com/watch?v=0RYETb9YVrk)

## NPMs weakness 
Early 2016 the npm community suffered a lot when the user Azer Koçulu removed more than 250 packages from NPM. Among thoose removed packages one in particular, left-pad, was used by many open source projects resulting in broken build processes all over. [How one developer just broke Node, Babel and thousands of projects in 11 lines of JavaScript
](http://www.theregister.co.uk/2016/03/23/npm_left_pad_chaos/). These kind of problems is now fixed and a developer now can´t remove code published in the npm community.

## Further reading
* https://docs.npmjs.com/
* [Kate Hudsons talk from nordic.js 2015](https://www.youtube.com/watch?v=0RYETb9YVrk)
