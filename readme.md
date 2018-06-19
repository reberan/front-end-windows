# Front-End Development Setup on a PC

This document assumes you're running a fresh copy of the latest version of **Windows 10**, >=version 1803.

The following workflow assumes a clean installation of Windows 10, whether from Signature PC or a full manual reinstall from a vendor laptop. While it's okay to have third-party software installed, the installation process will be more streamlined and less convoluted with a bare Windows 10 system.

- [Command Line Shell](#command-line-shell)
- [Windows Prepartion](#system-update-and-disk-encryption)
- [Projects Directory](#projects-directory)
- [.NET Framework](#net)
- [Windows Subsystem for Linux](#windows-subsystem-for-linux)
- [ZSH](#zsh-optional)
- [Chocolatey and Boxstarter](#chocolatey-and-boxstarter)
- [Ninite](#ninite)
- [Git](#git)
- [Node.js](#nodejs)
- [ES6](#es6)
- [Sass](#sass)
- [PHP and Composer](#php-and-composer)
- [Sublime Text, Atom, and VSCode](#sublime-text-atom-and-vscode)
- [VirtualBox](#virtualbox)
- [Vagrant](#vagrant)

## Command Line Shell

Throughout this document, you will encounter examples like this that contain one or more of the arguments listed:

    sudo command -flag --flag directory file.extention # Comments are behind pound signs

Front-end development has increasingly moved towards an open-source, command-line interface (CLI) dependent workflow. Whether we access modules, packages or simply useful commands, setting up a command-line shell to your liking is a good idea to start.

Anytime you see this, it is referring to your CLI of choice, whether it's the built-in Command Prompt, Powershell, Bash Shell in your Linux Distro or a third-party application like [Cmder](https://cmder.net/).

## System update and Disk Encryption

Step One - Update the system!
**Windows Key > Settings > Update & Security**

Step Two - Turn on BitLocker (Windows 10 Pro and above)
**Control Panel\System and Security\BitLocker Drive Encryption**

Click on the text "Turn on BitLocker" to turn on and enable BitLocker. On a brand new machine or Windows 10 installation, it will take some time to fully encrypt the disk.

Alternatively, you can use a third-party encryption software like [Veracrypt](https://en.wikipedia.org/wiki/VeraCrypt/), which is open-source and well regarded in the security community.

### Full Disk Encryption

Why do you want [full-disk encryption](https://en.wikipedia.org/wiki/Disk_encryption)? Theft.

You're most likely using a portable laptop of some kind. If you lose it, the laptop gets stolen or someone tries to hack into it, your personal data is at risk. Using full-disk encryption is an extra layer of security to keep your mind at ease in case of potential intrusion.

Two main caveats:
- Make sure you do not forget your BitLocker password. You'll have four options to retain a recovery key so choose the best option for you. Losing this recovery key means you cannot log in and everything on your computer is 100% inaccessible.
- After encryption completes, a corruption that makes the Windows 10 partition unaccessible has no recovery because of the level of encryption. Make sure you're both backing up using a local backup device such as [Windows 10 Backup](https://support.microsoft.com/en-us/help/17143/windows-10-back-up-your-files) on an external drive or a NAS, and a cloud backup provider like [Backblaze](https://www.backblaze.com/), [Carbonite](https://carbonite.com/), or [iDrive](https://www.idrive.com).

## Projects Directory

If you don't already have one, create a projects directory. I like to use `C:\Users\{yourusername}\Sites\[project-name]`. I prefer my Sites folder to exist along side the rest of my user profile folders.

    mkdir -p Sites

Depending on the type of projects you work on, this might not be necessary or preferable.

## .NET

An important dependency for application installation and IIS is **.NET Framework**.

**Control Panel > Turn Windows features on or off**

Choose to install, if not already installed, both .NET Framework 3.5 and .NET Framework 4.7.x.

## Windows Subsystem for Linux

Web development is not a sole operating system or browser process. WSL gives us more tools such as using native Unix commands within Windows. This is a big deal if you come from a Unix based OS and want to feel more comfortable on Windows.

Powershell.exe (Run as Administrator)

    Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Windows-Subsystem-Linux

After a restart, install your preferred version of Linux Distribution, Ubuntu being the most popular choice. Here's a [complete set of instructions](https://docs.microsoft.com/en-us/windows/wsl/install-win10) from Microsoft to install WSL.

Once installed, as you'll see below, we're going to use this VM to harbor the various languages and packages needed for web development. 

## ZSH (optional)

By default, Windows comes with three command line tools: Command Prompt, Powershell and now Bash in WSL. Powershell is a task automation and configuration management framework that includes command-line shell.

Because many tools recommended can be used with Bash, which contains the command language used to interact with Unix functions, such as `pwd`, `ls`, and `cd`, it is recommended to either use [Bash](https://docs.microsoft.com/en-us/windows/wsl/about) or use something like [Cmder](http://cmder.net/). Cmder is a console emulator that provides a shell for Bash.

[Z Shell](https://en.wikipedia.org/wiki/Z_shell), or ZSH, was written to extend Bash and make improvements to how Bash works. One of the most popular frameworks written around ZSH is called [Oh My Zsh!](http://ohmyz.sh/).

Install it using WSL:

    sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
    
Now you have ZSH installed. [Sign up and follow the videos recorded by Wes Bos](https://commandlinepoweruser.com/) to learn a ton more about ZSH and why it's so powerful.

## Chocolatey and Boxstarter

Package managers make it so much easier to install and update applications (for Operating Systems) or libraries (for programming languages). The most popular one for Windows is [Chocolatey](https://chocolatey.org/).

### Install

Two ways (Run as Administrator):

Cmd.exe

    C:\ @"%SystemRoot%\System32\WindowsPowerShell\v1.0\powershell.exe" -NoProfile -InputFormat None -ExecutionPolicy Bypass -Command "iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))" && SET "PATH=%PATH%;%ALLUSERSPROFILE%\chocolatey\bin"
    
For PowerShell, ([Ensure Get-ExecutionPolicy is at least RemoteSigned](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_execution_policies?view=powershell-6&viewFallbackFrom=powershell-Microsoft.PowerShell.Core)). 

PowerShell.exe 

    Set-ExecutionPolicy Bypass -Scope Process -Force; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))


### Usage

Chocolatey can be used with either `choco install` or `cinst`. They are interchangable.

To [install a package](https://chocolatey.org/docs/commands-install) simply type:

    choco install <package>
    choco install <package> --version=x.x.x # specific version (replace x with version number)

Replace `<package>` with the name of the package you want to install.

To view Chocolatey's directory of packages:

[https://chocolatey.org/packages](https://chocolatey.org/packages)

Other useful commands:

    choco upgrade <package>
    choco outdated # outdated packages
    choco list --a # what's installed, including version numbers

### Installing multiple applications

Chocolatey is awesome because now that you understand what it does, you can install all your favorite apps in one command! Here's a list of my favorite apps, including Google Chrome, that I need for development on a regular basis.

Note: if you use Boxstarter, you can include the following line inside your Powershell script and run everything together.

    choco install googlechrome chromium firefox opera brave vivaldi tor-browser thunderbird slack sublimetext3 atom vscode openvpn cmder notepadplusplus github sourcetree vlc filezilla virtualbox vagrant malwarebytes superantispyware qbittorrent authy-desktop boxstarter -y

Note: Google Chrome and Microsoft Edge contain Adobe Reader, Flash and Java by default. Running standalone versions of each is not recommended because they are a security risk without regular maintenance and updates.

### Boxstarter

The folks at Chcolatey provide new tool called [Boxstarter](http://www.boxstarter.org/) to automate a Windows setup process. The important reasons to use this tools are for resilient reboot and Windows customization.

    cinst boxstarter

Using boxstarter requires some step by step instructions to create an executable file all found on the website above. To get you started, I recommend starting with [this already created .ps1 file](https://gist.github.com/jessfraz/7c319b046daa101a4aaef937a20ff41f).

## Ninite

If you have more software you need installed not included in Chocolatey or you wish to use something that has a GUI, [Ninite](https://ninite.com/) is an awesome addition.

## Git

What's a developer without [Git](http://git-scm.com/)? To install, from your WSL command line run:

    sudo apt install git
    git config --global user.name "Your Name Here"
    git config --global user.email "your_email@youremail.com"
    git config --global core.autocrlf input

The `core.autocrlf=input` setting is pretty crucial; it can break things you install over git. If you have 2FA enabled on Github (you should), you’ll also need to follow the [Add SSH Key to Github](https://help.github.com/articles/adding-a-new-ssh-key-to-your-github-account/#platform-linux) and be sure you’re using the Linux instructions.

## Node.js

Node allows Node Package Manager (NPM) to install external scripts from a repository. Why is this important? You don't have to go to multiple websites to grab the individual library files. They can be automated into your project using NPM.

Install [Node.js](https://nodejs.org/) and [npm](https://npmjs.org/) with WSL command line:

    sudo apt install nodejs

Once installation is complete, restart your command line application so that you can verify that Node and NPM are correctly installed:

    node --version
    npm --version

Node modules are defined in a local `package.json` file inside your project. `npm install` will download external libraries and frameworks into each project's own `node_modules` folder by default. You'll never need to actually edit files in this folder, only reference them.

Two popular packages worth installing globally are [TypeScript](http://www.typescriptlang.org/), [Gulp](http://gulpjs.com/):

    npm install typescript
    npm install -g gulp

Your specific project might not need either of these but this is an example of how to install external libraries.

### Npm usage

To install a package:

    npm install <package> # Install locally
    npm install -g <package> # Install globally
    npm install <package> --save # Insert into package.json as dependency
    npm install <package> --save-dev # Insert into package.json as devDependency

To see what's installed:

    npm list # Local
    npm list -g # Global

Other useful commands:

    npm outdated # Outdated packages
    npm update <package> # Update a package
    npm uninstall <package> # Uninstall a package

## ES6

Javascript frameworks such as Angular, React and Vue now rely on the newest version of Javascript starting with ECMAScript 2015 (ES6). Browser quirks that gave rise to jQuery are less problematic because web standards are regularly implemented and iterated. Thus, the golden age of Javascript is upon us.

Until browsers catch up implementing the newest features of ES6, it is recommended to use a transpiler to convert your unsupported ES6 back to ES5, which is universally supported in all modern browsers.

The most popular transpiler is Babel(https://babeljs.io/). Only install this locally into an already created project using WSL command line:

    npm install --save-dev babel-cli babel-preset-env

Since it's generally a bad idea to run Babel globally you may want to uninstall the global copy by running `npm uninstall --global babel-cli`

## Sass

Install your preprocessor of choice, but I highly recommend using Sass. They all do the same thing but Sass has the most momentum behind it right now.

With WSL command line:

    npm install -g sass

Keep in mind that the utility that Sass offers is slowly being complimented and deprecated with the rise of [CSS variables](https://medium.freecodecamp.org/everything-you-need-to-know-about-css-variables-c74d922ea855).

## PHP and Composer

PHP is still one of the most used programming languages on the web, thanks in part to the amount of sites still using WordPress. We need a way to manage PHP scripts and packages similarly to how we manage JS dependencies using NPM.

One of the most popular PHP dependency managers is called [Composer](https://getcomposer.org/). The difference between Composer and NPM, for example, is that Composer works on a project-by-project basis, there is no global installations. So you must run and setup Composer on every new project if you want to use it.

To install Composer globally within Windows, go to the [Download page](https://getcomposer.org/download/) and run the package installer. It would be preferable to install this inside WSL instead of Windows.

## Sublime Text, Atom, and VSCode

The text editor is a developer's most important tool. Everyone has their preferences, but unless you're a hardcore [Vim](https://en.wikipedia.org/wiki/Vim_(text_editor)) user, I recommend one of three editors: [Sublime Text](https://www.sublimetext.com/), [VSCode](https://code.visualstudio.com/) or [Atom](https://atom.io/).

### Sublime Text

My preferred editor is Sublime Text 3.

    choco install sublimetext3

I prefer using the [beta version of Sublime Text 3](https://sublimetext.com/3) which is usually just as stable as version 2.

Sublime Text is not free, but it has an unlimited "evaluation period". The seemingly expensive $70 price tag is worth every penny. If you can afford it, I suggest you [support](https://www.sublimetext.com/buy) this awesome editor. :)

After installing Sublime Text, add [Package Control](https://packagecontrol.io/installation). This is the most important addition you'll make to Sublime Text and it'll give you the power to install plugins, add-ons, themes, color schemes and more.

#### Colors

I recommend to change two color settings:
- **Theme** (which is how the tabs, the file explorer on the left, etc. look)
- **Color Scheme** (the colors of the code).

My favorite theme is [Material Design Darker](https://equinsuocha.io/material-theme/#/darker) and a close second to [Seti_UI Theme](https://packagecontrol.io/packages/Seti_UI). 

Go to **Tools > Command Palette** (Shift-Command-P), Highlight **Package Control: Install Package** and then search for your preferred theme, make sure it's highlighted then press Enter to install it.

Then go to **Sublime Text > Preferences > Settings - User** and add the following two lines (using Seti UI) and restart Sublime:

    "theme": "Seti.sublime-theme",
    "color_scheme": "Packages/Seti_UI/Scheme/Seti.tmTheme",

#### Settings

Let's configure our editor a little. Go to **Sublime Text > Preferences > Settings - User** and paste this code from [my Preferences.sublime-settings file](https://gist.github.com/asuh/67586e056eba7757330f).

Feel free to tweak these to your preference. When done, save the file and close it.

[Let's create a shortcut so we can launch Sublime Text from the command-line](https://scotch.io/tutorials/open-sublime-text-from-the-command-line-using-subl-exe-windows) 
    
    1. Open Powershell as Administrator
    2. Type `sysdm.cpl` and press enter and go to System Properties > Advanced System Settings > Advanced > Environment Variables
    3. Create a New System Variable Create a new system variable called SUBLIME that will point to the folder of your Sublime installation.
    4. Add the System Variable to Your PATH Add the following to the end of your PATH variable: `;%SUBLIME%`

![Add System variable](https://i.imgur.com/pXTqfL8.jpg)


Now I can open a file with `subl myfile.html` or start a new project in the current directory with `subl .`. Pretty cool!

### Atom

If you like Sublime Text but can't afford or don't want to pay for it, [Atom](https://atom.io/) is an open-source editor in the spirit of Sublime Text that has a healthy community and regular updates.

The main problem for Atom as of autumn 2018 is that large files and projects noticeably slow down Atom's performance.

### VSCode

Visual Studio Code is 2018's "it" open-source code editor. There's a ton of great tutorials and articles, such as [VS Code Docs](https://code.visualstudio.com/docs/introvideos/basics) and [VS Code Can Do That?](https://vscodecandothat.com/).

## Virtualbox

The traditional way of setting up a front-end development environment used to be to work through FTP or directly on the server. WordPress installations still promote this workflow to some extent.

The last decade has seen significant changes to the front-end workflow, one of the most important being a local development environment: a local database, a local web server, and back-end language among other things. Using this environment decreases time and increases efficiency for front-end development.

There are several ways to setup a local development environment, whether it's installing a package like [MAMP](https://www.mamp.info/en/) or [XAMPP](https://www.apachefriends.org/index.html) or using virtual machines like [Virtual PC](https://www.microsoft.com/en-us/download/details.aspx?id=3702) to install the environments above.

The free, open-source alternative that I've been enjoying is called [Virtualbox](https://www.virtualbox.org/). This gives you a basic but very capable virtual machine host for any operating system that supports virtual installations.

    choco install virtualbox

Once installed, you can easily install many [versions of Internet Explorer from the Microsoft's VM site](https://developer.microsoft.com/en-us/microsoft-edge/tools/vms/).

## Vagrant

I have personally tried to move away from packaged development environments like MAMP. The alternative I've been enjoying a lot lately is called [Vagrant](https://www.vagrantup.com/). This gives you a powerful way to create a virtual and portable dev environment! It also has built-in connection to your local OS so that you develop in Windows but the environment runs in the VM.

    choco install vagrant

The brilliance of vagrant is its ability to be so portable. When you have a project you work with other developers, creating and destroying the identical dev environment is very simple, by reading a local vagrant instruction file. Once created, starting this environment is as simple as typing one command.

    vagrant up

My favorite box to use for new projects is called [Scotch Box](https://box.scotch.io/). It is fully-featured and contains everything I need built in to get started with many projects using PHP, JS or Ruby. For a WordPress environment, [Roots](https://roots.io/) has [Trellis](https://roots.io/trellis/) which includes everything you need for a powerful VM.

## TODO
- Allow apps downloaded from anywhere system preferences

## Credits

- [Front-End OS X](https://github.com/asuh/front-end-osx)
- [Webdev on Windows with WSL and VS Code](https://daverupert.com/2018/04/developing-on-windows-with-wsl-and-visual-studio-code/)
