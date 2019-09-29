HANDYMINER
2019 Alex Smith <alex.smith@earthlab.tech>

A simple wrapper for cBlake PoW-NG OpenCL Miner
to communicate with Handshake HSD via Stratum (hstratum)

```
       _.-._        _.-._
     _| | | |      | | | |_
    | | | | |      | | | | |
    | | | | |      | | | | |
    | _.-'  | _  _ |  '-._ |
    ;_.-'.-'/`/  \`\`-.'-._;
    |   '    /    \    '   |
     \  '.  /      \  .`  /
      |    |        |    |

```

EPIC Thanks to chjj and the entire Handshake Project

EPIC Thanks to the Handshake Alliance for being a solid team

EPIC Thanks to Steven McKie for being my mentor/believing in me

### BEFORE YOU GET STARTED WITH POW-NG

This current iteration is specific to the Next-Generation PoW work in Handshake. It will only be available on simnet as of writing this readme (9/27/2019) until we see the next testnet launch, at which time there will be updates. All the resources included in this particular distribution will be scoped to simnet network and handshake-org/hsd#pow-ng hsd fullnode. Using this on testnet 4 will not work for you. If you intent to mine to an existing fullnode you'll need a specific hsd branch, please see simnet_powng_fullnode_native_readme.md (or use the provided docker pow-ng fullnode)

### PREREQUISITES

[node.js](https://nodejs.org) v10.4 - v11+-ish (whatever one has bigint support)

(optional) [docker](#dockerReminders) if you want to run your own fullnode to mine to with the provided utilities

(windows) [git bash](https://git-scm.com/downloads) A handy bash terminal

(windows) [mingw-64 8.1.0 with gcc aka "MinGW-W64-install.exe"](https://sourceforge.net/projects/mingw-w64/files/Toolchains%20targetting%20Win32/Personal%20Builds/mingw-builds/8.1.0/threads-posix/dwarf/) bash terminal utilities, please add the installation /bin folder to your Path environment variable

### INSTALLATION

Linux: OpenCL Drivers and dependencies install can be found in [./linux_installation.md](./linux_installation.md)

```npm install``` in this directory or (windows) double-click ```install.windows.bat``` (and make sure to run this as administrator)

Note: When you run the npm install we will try to install a copy of hsd in this directory. It will probably fail/complain about unbound support which is fine. We won't be running HSD anyway, only including a couple of files for block header creation from actual HSD block data. You can ignore warnings/failures in this install.

Windows folks: If you didnt double click ```install.windows.bat``` youll need to run the following commands in the repo root (make sure to use admin privileges to the terminal) :
```npm install --global --production windows-build-tools && npm config set msvs_version 2017 --global``` followed by your ```npm install```

Windows may also need to add the following two items added to the ```Path``` environment variable:
```
C:\Program Files\nodejs\node_modules\npm\bin
C:\Program Files (x86)\Microsoft Visual Studio\2017\BuildTools\MSBuild\15.0\Bin
```

### MINER CONFIGURATION

The first time you run the miner, you will run through a configurator which will write a config to ./config.json. Should you want to reconfigure in the future, you can just run ```node configure.js``` in this directory. Alternately delete config.json and (re)start the miner.

Required Items to have ready for configuration:

0. host or IP address of the pool or solo node you mine to (127.0.0.1 for your local fullnode)

Optional configuration items (just leave blank and hit enter if you dont know) :
1. pool or solo node port (probably 3008)
2. the stratum password (optional)
3. the wallet you want to mine to (pool only)

### THE POINT

Non-terminal users: simply double click ```(windows) dashboard.windows.bat``` or ```(mac) dashboard.mac.command``` or ```(linux) dashboard.sh``` files

Launch the miner in terminal like:
```
cd (into the repo base)
npm start (runs the CLI miner)

Most terminals:
npm run dashboard //runs the CLI dashboard+miner
OR
./dashboard.sh //same

Note: many windows terminals dont do text coloring or dashboards right with npm run commands.. So if it's an issue (and you didnt double click) you can launch the proper dashboard like:

node --max-old-space-size=8196 ./miner/dashboard.js
```

Note: Mac users need to enter their password to use the dashboard utility. We're not doing anything weird, MacOS requires it to ask the system for temperature/fan information.

Mine blocks!

(Ctrl+C to stop the miner)

### Running an HSD Fullnode for solo mining

There are some handy docker utilities in the folder ```./fullnode_utils``` which you can double click on mac/windows/linux to create, launch, stop or nuke a stratum-enabled fullnode on your local machine that you can mine to. Any utilities speicific to the POWNG-simnet tests are noted in the command names. 

<a id="dockerReminders"></a>
#### Docker Fullnode Reminders:
1. Make sure you installed docker. PSA: you dont need to login to docker either, dont let them fool you [mac, hunt for the dmg link](https://docs.docker.com/docker-for-mac/release-notes/) [windows, hunt for the exe link](https://docs.docker.com/docker-for-windows/release-notes/) ). Even after you install it: you dont need to login in order to run this goodness.
2. Make sure that you added your wallet address you'd like paid at to ```run.mac.command OR run.windows.bat OR run.sh (for production)``` or ```run.powng.mac.command OR run.powng.windows.bat OR run.powng.sh (for simnet powng testing rn)``` in the end of the command that looks like: ```"./run.sh hs1qu2zqenh8jdxvaqz7nwun4r3k32klmuf6ss2y9s simnet"```

#### Docker Fullnode FAQs:

1. I get an error:: ``` Error response from daemon: driver failed programming external connectivity on endpoint earthlabHSD (...) Error starting userland proxy: mkdir /port/tcp:0.0.0.0:14038:tcp:172.17.0.2:14038: input/output error Error: failed to start containers: earthlabHSD ``` 
Right click the docker icon in your taskbar and 'restart docker'. It might take a couple minutes to fully restart.

2. Can I access the hsd docker fullnode? 
Yes. Port 15937 will be open for RPC calls to the node per usual, else ssh into the container's hsd directory with::
```docker exec -it earthlabHSD bash```
Windows:
```winpty docker exec -it earthlabHSD bash```

3. I'd like to import my wallet into this fullnode, How do I set my fullnode's API password and/or stratum password to something more meaningful? 

Edit the file you run the fullnode with, ```run.mac.command``` or ```run.windows.bat``` for example, and add the 3rd and 4th parameters into run.sh like:::
```docker start earthlabHSD && docker exec -i earthlabHSD sh -c "./run.sh hs1qu2zqenh8jdxvaqz7nwun4r3k32klmuf6ss2y9s main my_hsd_api_password_here my_stratum_password_here"``` 
and pay attention that the passwords are inside that double quote ^^

4. If you don't want to run a Dockerized fullnode, you can feel free to check the docs in ```simnet_powng_fullnode_native_readme.md``` to see how to launch your own native fullnode that is on simnet and pow-ng.