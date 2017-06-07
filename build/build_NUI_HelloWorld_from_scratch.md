<a name="0"></a>
# Setup NUI development environment on Ubuntu

This guide explains how to setup, build and run NUI (Dali C#) applications using Visual Studio Core.

This document covers:

[Installation](#1)<br>
[Getting NUI source code](#2)<br>
[](#3)<br>
[](#4)<br>
[](#5)<br>
[](#6)<br>
[](#7)<br>
[](#8)<br>

## Overview
It assumes the starting point is a completely 'clean' system.

The [Hello World tutorial](../NUItutorials/hello-world.md) provides an introduction into NUI application development, describing how to display text in a text label.

## Step-by-step guide

<a name="1"></a>
### Installation
+ Install dot net core for Ubuntu
Follow instructions on [installing dotnet core for Ubuntu](www.microsoft.com/net/core#linuxubuntu)
Follow instructions on [FROM MICROSOFT](https://www.microsoft.com)

+ Install latest Visual Studio Core (VSC) for Ubuntu
a. [Download deb package from](https://code.visualstudio.com)
b. Install deb package with:
~~~{.bash}
    $ sudo dpkg -i code_1.10.2xXXXXXXXXXX_amd64.deb
~~~

+ Open VSC
a. In the desktop launcher, select *Search your Computer* > *Applications* for the Visual Studio Code icon
b. Copy VSC icon to Launcher
c. Select *Launch* to open VSC, or double click on VSC icon in Launcher

+ Setup Proxy settings for VSC _to enable install and download of extensions and libraries_

	a.   	Configure System firewall proxy settings. (To enable installation of the VSC _C# extension_) 
		On desktop, select System Settings > Network > Network Proxy >
        		HTTP Proxy : http://106.1.18.35	8080
       			HTTPS Proxy: http://106.1.18.35	8080

	b.   	Configure firewall proxy settings in VSC. (To enable installation of library packages such as mono runtime and .NET Core Debugger) 
		Select File > Preferences > Settings > Edit
		Select HTTP in middle pane
		Select Edit icon > Copy to settings. "http.proxy" should be copied to right hand pane :-
~~~{.bash}
		{
        		"http.proxy":
		}
~~~

		Add the proxy setting:
~~~{.bash}
		{
       			"http.proxy": "http://106.1.18.35:8080
		}

		The proxy settings are saved to the settings.json file.
~~~

	OR

	c.	Set the OS environment variables http_proxy and https_proxy as above.

+ Install C# Extension in VSC
	a.      Install from within VSC. Bring up the Extensions view by clicking on the extensions icon in the Activity Bar,
		or Ctrl+Shift+X (View extensions command). This will bring up all the extensions in the VS code marketplace.
		Click the Install button next to C#. After a succesful install, you will see the Reload button, click to restart VSC.
		(_Alternatively Install _C# extension_ from [](https://marketplace.visualstudio.com)_)

	a.   	Setup firewall proxy settings in VSC.
		Select File > Preferences > Settings > Edit "http.proxy" (_in settings.json_).
        	e.g "http.proxy": "http://106.1.18.35:8080

#### Recommended - Familiarise with VSC
+ Build VSC with a console hello world

![This tutorial](https://docs.microsoft.com/en-us-dotnet/csharp/getting-started/with-visual-studio-code)

will give you a basic understanding of building, debugging and running projects in VSC.

<a name="2"></a>
### Get NUI source code from Git
+ Get code from git (_review.tizen.org_ server)

a.	Create a 'NUI root folder' for the source code e.g. mkdir ~/DALiNUI (_example folder used for rest of the tutorial_)

b.
~~~{.sh}	
	$ cd ~/DALiNUI
~~~

c.  	Get code:
~~~{.sh}
	$ git clone ssh://[your account]@review.tizen.org:29418/platform/core/uifw/dali-core
	$ git clone ssh://[your account]@review.tizen.org:29418/platform/core/uifw/dali-adaptor
	$ git clone ssh://[your account]@review.tizen.org:29418/platform/core/uifw/dali-csharp-binder
	$ git clone ssh://[your account]@review.tizen.org:29418/platform/core/uifw/dali-toolkit
	$ git clone ssh://[your account]@review.tizen.org:29418/platform/core/csapi/nui
~~~

where 'your account' is 'm.castling' in my case, so-  ...://m.castling@review.tizen.org:29418.....

Observation: _only the .git file is pulled into the _dali-csharp-binder_ and _nui_ folders.

+ Switch to the 'devel master' branch for each required repo

~~~{.sh}
a.	$ cd ~/DALiNUI/dali-core

b.	$ git checkout devel/master

c.	$ git pull
~~~

Repeat above steps for the dali-adaptor and dali-toolkit folders.

[Back to top](#0)

<a name="4"></a>
### Build NUI environment
+ Build environment setup, saving to a file

~~~{.sh}
a.	$ cd ~/DALiNUI

b.	$ dali-core/build/scripts/dali_env -c

c.	$ dali-env/opt/bin/dali_env -s > setenv

d.	$ . setenv
~~~

These steps only need to be done once.

You will have to source your environment variables every time you open up a new terminal (or you can add to .bashrc if you prefer).
You can do this by sourcing the ''setenv'' script you created above: 

~~~{.sh}
	$ . setenv
~~~

### Build NUI source code
+ Build DALi *native* repo's

To build, follow instructions in the README file in each repo folder.

a.	Build in the following order:
		dali-core
		dali-adaptor
		dali-toolkit

_The shared library files (.so) will be built and installed into the ~/DALiNUI/dali-env/opt/lib_ folder.

b.	To subsequently clean the build, use:
~~~{.sh}
	$ make maintainer-clean	
~~~

+  Run and debug DALi native (*Optional* step)

This step can be done if required to test native build.

This step requires the _dali_demo_ repo.

Repeat above steps for getting and building dali_demo repo:

a.	Get code:
~~~{.sh}
	$ git clone ssh://[your account]@review.tizen.org:29418/platform/core/uifw/dali-demo
~~~
b. 	Switch to 'devel master' branch

c. 	Build from README

d. 	run:
~~~{.sh}
	$ cd ~/DALiNUI/dali-env/opt/bin
	$ . setenv
	$ dali-demo
~~~

If ok, DALi demo window will appear.

+ Build NUI csharp bindings

In this step we build the C# bindings.

a.
~~~{.sh}
	$ cd dali-csharp-binder
~~~

b. 	Edit _file.list_ and remove the line "src/key-grab.cpp \".
   	(_This is a tizen only dependency_). Do not leave a gap in the file.

c. 	Build bindings by following the README file. (_"Building the Repositry"_)

+ Overwrite existing NUI files

Create a sub folder (_I have used nuirun_), copy nui source code into sub folder:
~~~{.sh}
a. 	$ cd ~/DALiNUI

b.   	$ mkdir nuirun

c.	$ cp -r nui/Tizen.NUI/src nuirun
~~~

Overwrite *NUIApplication.cs* and *CoreApplication.cs* files in ~/DALiNUI/nuirun/src/public with
the same named files in this gerrithub repositry (TBD). If access to *Confluence* is available, these files can
alternatively be obtained from the 'How to article'- "Building NUI (DALi C#) using Visual Studio COnfluence".

_This step is necessary as NUI In Ubuntu is not fully supported just yet._

+ Copy shared library

~~~{.sh}
cp  dali-env/opt/lib/libdali-csharp-binder.so ~/DALiNUI/nuirun/bin/Debug/netcoreapp1.1/
~~~

+ Modify project file

### Build and Run the Hello World NUI Tutorial

+ Create Hello World project in VSC

a. Open the command prompt (Ctrl+`) > TERMINAL, and type the following:

~~~{.sh}
b. 	$ cd ~/DALiNUI/nuirun

c.	$ . setenv

d.   	$ dotnet new console
~~~

The setenv may not be necessary, depending on how the environment has been setup.

The _dotnet new console_ creates a PROJECT file *nuirun.csproj* which is essential,
and also creates Program.cs, which should be deleted in VSC Explorer.

+ Delete Program.cs

+ Modify project file

a. Edit nuirun.csproj, adding the following line inside the PropertyGroup element:

	<DefineCOnstants>DOT_NET_CORE<DefineCOnstants>

+ Create tutorial file (in VSC)

a.	File > New File

b. 	Copy code in (_"full example" section_) of ![Hello World](./hello-world.md) to new file

c. 	Rename file - File > Save As "hello-world.cs", or select right click Rename from menu

+ Build tutorial

a. Resolve the build assets
	
~~~{.sh}
	$ dotnet restore
~~~

Running restore pulls down the required packages

+ Configure VSC by creating task.json
a.	Press Ctrl+Shift+P to open the command Pallete, type "ctr", and select Configure Task runner > NET core

This is essential, or else will get "No task runner configured" or "Error Could not find the Pre Launch Task 'build'"
message pane on building. (_missing task.json file_).

b. build
~~~{.sh}
	$ dotnet build
~~~


### Modify Hello World Application size
In VSC, Open launch.json via the Explorer. In the "configurations" section, add the required width and height:

~~~{.sh}
 	"name": ".NET Core Launch (console)",

	...
	...
            "env": {
                "DALI_WINDOW_WIDTH":"600",
                "DALI_WINDOW_HEIGHT":"800"
            },
~~~

[Back to top](#0)

