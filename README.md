# Mirror-Bot
AI Mirror
Known Vulnerabilities

MagicMirror² is an open source modular smart mirror platform. With a growing list of installable modules, the MagicMirror² allows you to convert your hallway or bathroom mirror into your personal assistant. MagicMirror² is built by the creator of the original MagicMirror with the incredible help of a growing community of contributors.

MagicMirror² focuses on a modular plugin system and uses Electron as an application wrapper. So no more web server or browser installs necessary!
Table Of Contents

    Usage
    Configuration
    Modules
    Known Issues
    Community
    Contributing Guidelines

Usage
Raspberry Pi Support

Electron, the app wrapper around MagicMirror², only supports the Raspberry Pi 2 & 3. The Raspberry Pi 1 is currently not supported. If you want to run this on a Raspberry Pi 1, use the server only feature and setup a fullscreen browser yourself.
Automatic Installer (Raspberry Pi Only!)

Execute the following command on your Raspberry Pi to install MagicMirror²:

bash -c "$(curl -sL https://raw.githubusercontent.com/MichMich/MagicMirror/master/installers/raspberry.sh)"

Manual Installation

    Download and install the latest Node.js version.
    Clone the repository and check out the master branch: git clone https://github.com/MichMich/MagicMirror
    Enter the repository: cd ~/MagicMirror
    Install and run the app: npm install && npm start

Important: npm start does not work via SSH, use DISPLAY=:0 nohup npm start & instead. This starts the mirror on the remote display.

Note: if you want to debug on Raspberry Pi you can use npm start dev which will start the MagicMirror app with Dev Tools enabled.
Server Only

In some cases, you want to start the application without an actual app window. In this case, you can start MagicMirror² in server only mode by manually running node serveronly or using Docker. This will start the server, after which you can open the application in your browser of choice. Detailed description below.
Docker

MagicMirror² in server only mode can be deployed using Docker. After a successful Docker installation you just need to execute the following command in the shell:

docker run  -d \
			--publish 80:8080 \
			--restart always \
			--volume ~/magic_mirror/config:/opt/magic_mirror/config \
			--volume ~/magic_mirror/modules:/opt/magic_mirror/modules \
			--name magic_mirror \
			bastilimbach/docker-magicmirror

Volumes 	Description
/opt/magic_mirror/config 	Mount this volume to insert your own config into the docker container.
/opt/magic_mirror/modules 	Mount this volume to add your own custom modules into the docker container.

You may need to add your Docker Host IP to your ipWhitelist option. If you have some issues setting up this configuration, check this forum post.

var config = {
	ipWhitelist: ["127.0.0.1", "::ffff:127.0.0.1", "::1", "::ffff:172.17.0.1"]
};

If you want to run the server on a raspberry pi, use the raspberry tag. (bastilimbach/docker-magicmirror:raspberry)
Manual

    Download and install the latest Node.js version.
    Clone the repository and check out the master branch: git clone https://github.com/MichMich/MagicMirror
    Enter the repository: cd ~/MagicMirror
    Install and run the app: npm install && node serveronly

Raspberry Configuration & Auto Start.

The following wiki links are helpful in the configuration of your MagicMirror² operating system:

    Configuring the Raspberry Pi
    Auto Starting MagicMirror

Updating your MagicMirror²

If you want to update your MagicMirror² to the latest version, use your terminal to go to your Magic Mirror folder and type the following command:

git pull && npm install

If you changed nothing more than the config or the modules, this should work without any problems. Type git status to see your changes, if there are any, you can reset them with git reset --hard. After that, git pull should be possible.
Configuration

    Duplicate config/config.js.sample to config/config.js. Note: If you used the installer script. This step is already done for you.
    Modify your required settings.

Note: You'll can check your configuration running the follow command:

npm run config:check

The following properties can be configured:
Option 	Description
port 	The port on which the MagicMirror² server will run on. The default value is 8080.
address 	The ip address the accept connections. The default open bind :: is IPv6 is available or 0.0.0.0 IPv4 run on. Example config: 192.168.10.100.
ipWhitelist 	The list of IPs from which you are allowed to access the MagicMirror². The default value is ["127.0.0.1", "::ffff:127.0.0.1", "::1"]. It is possible to specify IPs with subnet masks (["127.0.0.1", "127.0.0.1/24"]) or define ip ranges (["127.0.0.1", ["192.168.0.1", "192.168.0.100"]]). Set [] to allow all IP addresses. For more information about how configure this directive see the follow post ipWhitelist HowTo
zoom 	This allows to scale the mirror contents with a given zoom factor. The default value is 1.0
language 	The language of the interface. (Note: Not all elements will be localized.) Possible values are en, nl, ru, fr, etc., but the default value is en.
timeFormat 	The form of time notation that will be used. Possible values are 12 or 24. The default is 24.
units 	The units that will be used in the default weather modules. Possible values are metric or imperial. The default is metric.
modules 	An array of active modules. The array must contain objects. See the next table below for more information.
electronOptions 	An optional array of Electron (browser) options. This allows configuration of e.g. the browser screen size and position (example: electronOptions: { fullscreen: false, width: 800, height: 600 }). Kiosk mode can be enabled by setting kiosk = true, autoHideMenuBar = false and fullscreen = false. More options can be found here.
customCss 	The path of the custom.css stylesheet. The default is css/custom.css.

Module configuration:
Option 	Description
module 	The name of the module. This can also contain the subfolder. Valid examples include clock, default/calendar and custommodules/mymodule.
position 	The location of the module in which the module will be loaded. Possible values are top_ bar, top_left, top_center, top_right, upper_third, middle_center, lower_third, bottom_left, bottom_center, bottom_right, bottom_bar, fullscreen_above, and fullscreen_below. This field is optional but most modules require this field to set. Check the documentation of the module for more information. Multiple modules with the same position will be ordered based on the order in the configuration file.
classes 	Additional classes which are passed to the module. The field is optional.
header 	To display a header text above the module, add the header property. This field is optional.
disabled 	Set disabled to true to skip creating the module. This field is optional.
config 	An object with the module configuration properties. Check the documentation of the module for more information. This field is optional, unless the module requires extra configuration.
Modules

The following modules are installed by default.

    Clock
    Calendar
    Current Weather
    Weather Forecast
    News Feed
    Compliments
    Hello World
    Alert

For more available modules, check out out the wiki page: MagicMirror² Modules. If you want to build your own modules, check out the MagicMirror² Module Development Documentation and don't forget to add it to the wiki and the forum!
Known issues

    Electron seems to have some issues on certain Raspberry Pi 2's. See #145.
    MagicMirror² (Electron) sometimes quits without an error after an extended period of use. See #150.
