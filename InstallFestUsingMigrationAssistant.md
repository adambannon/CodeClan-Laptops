# Installfest using Migration Assistant


### Master copy
Ensure a machine is available to clone. This should be set up as per the instructions below.

### Machine to be setup: Wipe drive and reinstall OS X
Skip this if it's a new machine

Press the power button. As soon as you hear the bing, press Cmd and R
In Disk Utilities, Erase the Hard Drive
Reinstall OS X


### Machine to be setup: Run updates
Ensure the OS is the same version as the machine being copied.  Run any updates.


### Connect and copy
Connect the two machines using the Thunderbolt cable.  
Open Migration Assistant on both machines.
On the machine to be copied, select 'To another Mac'. 
On the machine to be setup, select 'From a Mac, Time Machine or startup disk'
Select the machine being copied from the machine being setup and then select Continue lots.  
When asked, replace the User account with the one being copied. **NOTE** make sure the "Keep a copy of the data from this Mac ..." option is **unchecked** . 

### After Migration

####Sanity Checks
**These checks MUST be carried out before doing anything else to make sure that the migration has worked as expected**

1. Make sure that Oh-My-Zsh has installed properly by opening a terminal. The terminal should open without any error messages and the prompt should be a tilde and in color (usually green).
2. Open PostgreSQL from the launcher. You should see a little elephant on the main menu bar (near or next to the wifi icon).
3. Right click on the elephant and click on ```Preferences...```. Check the  `Start Postgres automatically after login` option and uncheck the  `Show Welcome Window when Postgres starts` option.
4. Log out and re-login - Postgres should have started automatically i.e. the elephant icon should be on the main menu bar.

**Make sure that the above steps all run successfully before proceeding any further**

####Final set up
1. In System Preferences / Sharing rename the new machine to be CODECLANxx for a Student machine (based on the computer asset number underneath the machine) or XXXX's MAcbook Pro for a staff machine
2. In System Preferences / General deselect 'Allow Handoff'  
3. In System Preferences / Security & Privacy enable location services
4. In System Preferences / iCloud make sure that Find my Mac is enabled.  Photos is the only other item selected, all others should be deselcted
5. Remove unnecessary items from the Dock (like any extra Downloads folder, Photos, Maps)
6. Clean the physical machine (particularly keyboard and screen) and hard cover
7. Make sure all the items in the box are labeled with the asset number (both power plugs and the transformer)
8. Label the box with a postit with the asset number and 'Ready for Student/Staff use' as applicable


# Installfest from scratch


### Wipe drive and reinstall OS X
Skip this if it's a new machine

Press the power button. As soon as you hear the bing, press Cmd and R
In Disk Utilities, Erase the Hard Drive
Reinstall OS X

Clean the physical machine (particularly keyboard and screen) and hard cover


### Pre-install

Set up the machine with a new user called User but don't sign in to Apple at this stage

1. Identify which version of OSX you're using - ideally you should have El Capitan (10.11.x).  If not, upgrade the OS in App Store.  
2. Install any OS updates displayed in the App Store
3. Ensure that you've uninstalled any antivirus software you may have, as it can prevent some of the tools from installing properly

Set the desktop background to the CodeClan image and the user profile pic to be the CodeClan logo, both saved in Photos


### Install XCode

https://itunes.apple.com/us/app/xcode/id497799835?ls=1&mt=12

This will take a while to install. You can continue on with the rest of the instructions while it's installing


### Install and connect to HipChat

https://www.hipchat.com/downloads

Install the OSX app and keep it running


### Install Google Chrome
(can be done while XCode is installing)

1. Go to [https://google.com/chrome](https://google.com/chrome)
2. Click on `Download Chrome`
3. Go to the Downloads folder and run the `googlechrome.dmg` package
4. Go to System Preferences->General->Handoff & Selected Apps and uncheck the 'Allow Handoff between This Mac and your iCloud devices'. This will stop the 'Google Chrome from Mac' icon appearing in the Docking bar.

### Install Sublime Text 3
(can be done while XCode is installing)

1. Download and install [Sublime Text 3](http://www.sublimetext.com/3) - version 3083 is current
2. Create a `bin` directory in the home directory to hold symlinks etc:
`mkdir $HOME/bin`
3. Make a symlink for Sublime Text, allowing us to launch it from the command line:
`sudo ln -s /Applications/Sublime\ Text.app/Contents/SharedSupport/bin/subl $HOME/bin/subl`
4. Put the newly created `bin` directory in your path for now, so you can use the symlink before installing zsh:
`export PATH=$HOME/bin:$PATH`

### Install Command Line Tools
(can be done while XCode is installing)

1. Run from the command line: `xcode-select --install`
2. Choose `install tools` from the prompt and `agree` to the terms
3. If you recieve a message saying "Can't install the software because it is not currently available from the Software Update server"... it's probably because the command line tools are already installed...
4. Agree to the license by typing `sudo xcodebuild -license`
5. Press enter, then `q`
6. Then on the next prompt, type `agree`


### Install Brew Package Manager

(Brew allows us to install and update software (like Ruby, Git, MongoDB, etc) from the command line)

1. Open http://brew.sh/, scroll down to Install Homebrew and copy+paste the command into the terminal.
2. Ensure that Homebrew is raring to brew and fix any issues:
`brew doctor`
3. Update Homebrew:
`brew update`

(nb. the absolute paths will not be used after the next step, but might not be needed if they already have `/usr/local/bin` in their $PATH)


### Install Zsh
(shell is a user interface for access to an operating system's services)
(bash stands for 'bourne again shell', zsh stands for 'z-shell')

1. Type `brew install zsh`
2. Type `zsh`. Type `0` if the prompt ask you about .zshrc . You should have a different prompt
3. Type `exit` to return to bash
4. Type `which zsh` to determine where your new shell has installed. This shows `/YOUR/PATH/TO/zsh` (usually `/usr/local/bin/zsh`)
5. Type `$HOME/bin/subl /etc/shells` and add `/YOUR/PATH/TO/zsh`. (Lists trusted shells. The chsh command allows users to change their login shell only to shells listed in this file)
6. In a new tab, type `chsh -s /YOUR/PATH/TO/zsh`, then close and reopen your terminal application to This will enable zsh by default.
7. Type `echo $SHELL`. this should return `/YOUR/PATH/TO/zsh`


### install Oh-My-Zsh
(The PATH environment variable is a colon-delimited list of directories that your shell searches through when you enter a command)

1. Type `curl -L https://github.com/robbyrussell/oh-my-zsh/raw/master/tools/install.sh | sh`
2. Let's move Homebrew's directory further up the PATH, so that software loads from there by preference:
  * Open the zshrc file `$HOME/bin/subl ~/.zshrc`
  * Find the line that starts with something like ```export PATH=``` (usually near the end of the file)
  * Change it so it looks like the following: ```export PATH="/usr/local/bin:$HOME/bin:$PATH"```
  * While you're in here, disable auto-updating by removing the `#` in front of the setting (around line 18)
  * Add the following two lines to the bottom of the file to make Sublime the default text editor:

    ```
     export EDITOR='subl -w -n'
     export PAGER='less -f'
    ```

Close your terminal and open a new one - the prompt should be an arrow and tilde, and in colour.


### Install Ruby Environment (Rbenv) and builder

1. Install rbenv (the Ruby version manager) and ruby-build (the Ruby version builder) from Homebrew:
`brew install ruby-build rbenv`
2. Ensure that rbenv is loaded whenever we open a command line session:
`echo 'if which rbenv > /dev/null; then eval "$(rbenv init -)"; fi' >> ~/.zshrc`
3. Move the rbenv environment higher in the load order:
`echo 'export PATH=$HOME/.rbenv/shims:$PATH' >> ~/.zshrc`
4. Quit the current terminal (Cmd+Q)
5. And then reopen the terminal application, ensuring there are no errors - you should be able to type `rbenv -v` and get a version number.


### Install a version of Ruby with ruby-build
(Yukihiro Matsumoto wrote ruby, Matz's Ruby Interpreter or Ruby MRI)

1. OS X 10.9 comes with Ruby baked in, but it's version is not the latest one. We could upgrade that version of Ruby to a newer one, but what if we needed to run one version of Ruby for one app, and a different version for another?
2. Install Ruby 2.3.0. This is the latest stable version of Ruby:
`rbenv install 2.3.0`
3. This is required every time we install a new Ruby or install a gem with a binary:
`rbenv rehash`
3. Set the global Ruby to the one we've just installed, which is a sensible default:
`rbenv global 2.3.0`
4. Test you have the right version with `ruby -v`


### Skip gem rdoc generation

1. Whenever we install a gem, it also installs a bunch of documentation we probably don't want - the following command allows us to avoid this:
`echo 'gem: --no-rdoc --no-ri' >> ~/.gemrc`


### Install Bundler

1. Bundler manages Ruby gems per-project/application, and makes it trivial to install all the dependencies for an application: `gem install bundler`
2. `rbenv rehash` (because we've installed a new binary)


### Install Wget

1. We will often download files from the internet in the command line... this is the tool to do that:

  `brew install wget`

### Install node and npm

`brew install node`

### Install PostgreSQL

1. Download [PostgreSQL](http://postgresapp.com/)
2. Unzip the downloaded file and drag to `applications`
3. Choose `automatically start at login` in preferences, and don't `show welcome screen`
4. Update the PATH variable in .zshrc. To do this run the following in the command line: 
`echo 'export PATH="/Applications/Postgres.app/Contents/Versions/9.5/bin:$PATH"' >> ~/.zshrc`


### Install Git

1. This ensures we can upgrade Git more easily:
`brew install git` `rehash`
2. Ensure you're not using "Apple Git" from the path `/usr/bin/git` by checking `which git` and `git --version`

### STAFF MACHINES ONLY - Login to Git
(**To be done only by instructors when setting up their own machine - students will have to do this for themselves later**)
Configure your name and email address for commits (be sure to use the email address you have registered with Github):
```
  git config --global user.name "Your Name"
```
```
  git config --global user.email "you@example.com"
```


### Globally ignore 'DS_Store' files

Since we never want to track .DS_Store files, we can make a global `.gitignore` file, and tell git to use it for all repositories:

```
  echo .DS_Store >> ~/.gitignore_global
```
```
  git config --global core.excludesfile ~/.gitignore_global
```

### Globally ignore 'public/uploads/' contents

Add this to never track the contents of your uploads folder in Rails.

```
  echo /public/uploads/ >> ~/.gitignore_global
```

### STAFF MACHINES ONLY - Configure SSH access to Github
**NOTE To be done only by instructors when setting up their own machine - students will have to do this for themselves later**

1. First, we need to check for existing SSH keys on your computer. Open up your Terminal and type:
`ls -al ~/.ssh`
Check the directory listing to see if you have files named either id_rsa.pub or id_dsa.pub. If you have either of those files you can skip to the step 'add your SSH key to Github'.
2. Generate a new SSH key
`ssh-keygen -t rsa -C "your_email@example.com"`
3. You'll be prompted for a file to save the key, and a passphrase. Press enter for both steps (default name, and no passphrase)
4. Then add your new key to the ssh-agent:
`ssh-add ~/.ssh/id_rsa`
5. Add your SSH key to GitHub by logging into Github, visiting `account settings` and clicking `SSH keys`. Click `Add SSH key`
6. Copy your key to the clipboard with the terminal command:
`pbcopy < ~/.ssh/id_rsa.pub`
7. On Github, create a descriptive title for your key, an paste into the `key` field - *do not add or remove and characters or whitespace to the key*
8. Click `Add key` and check everything works in the terminal by typing:
`ssh -T git@github.com`


### Clone repositories for assignments and classnotes
TODO: fill in details of the repositories...


### Accessibility

Install Open Dyslexic font https://gumroad.com/l/OpenDyslexic
Unzip OpenDyslexic3 and Mono and copy the Font files to the Fonts folder (or open Font Book and select Add Fonts)


### Customize Sublime

#### Install the [sublime package manager](https://packagecontrol.io/installation)

* To install the package manager, press `ctrl + ``
* then copy/paste this command (yes, the whole thing!) into the newly opened Sublime terminal:

```
import urllib.request,os,hashlib; h = '2915d1851351e5ee549c20394736b442' + '8bc59f460fa1548d1514676163dafc88'; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); urllib.request.install_opener( urllib.request.build_opener( urllib.request.ProxyHandler()) ); by = urllib.request.urlopen( 'http://packagecontrol.io/' + pf.replace(' ', '%20')).read(); dh = hashlib.sha256(by).hexdigest(); print('Error validating download (got %s instead of %s), please try manual install' % (dh, h)) if dh != h else open(os.path.join( ipp, pf), 'wb' ).write(by)
```
(If this does not work then use the command under 'Sublime Text 3' in the link above)

Press `cmd-shift-p` and type 'Package Control: Install Package'

- Install "GitGutter"
- And ["Maybs Quit](https://sublime.wbond.net/packages/Maybs%20Quit")
- SideBarEnhancements
- SublimeLinter
- SublimeLinter-Ruby

#### Reindent shortcuts

Add _reindent_ and _paste_and_indent_ shortcuts in Preferences/Key Bindings - User, restart sublime:

```
  [
    { "keys": ["super+v"], "command": "paste_and_indent" },
    { "keys": ["super+shift+v"], "command": "paste" },
    { "keys": ["super+shift+r"],  "command": "reindent" }
  ]
```


#### Set preferences

`cmd + ,` allows you to access the sublime's preferences.

```
  {
    "auto_complete_commit_on_tab": true,
    "find_selected_text": true,
    "font_face": "Monaco",
    "highlight_active_indent_guide": true,
    "highlight_line": true,
    "highlight_modified_tabs": true,
    "tab_size": 2,
    "translate_tabs_to_spaces": true,
    "hot_exit": false,
    "word_wrap": true
  }
```

**NOTE** the 'OpenDyslexic3' font can be used instead of 'Monaco' if students require/wish it.
 
### Move the hash symbol somewhere more convenient

1. Bind the `§` key to the hash symbol by creating/editing the default key binding dictionary:

```
  mkdir ~/Library/KeyBindings
  subl ~/Library/KeyBindings/DefaultKeyBinding.dict
```

2. Add the binding:

```
  {
    /* Map # to § key*/
    "§" = ("insertText:", "#");
  }
```

3. Restart your Mac for this to take effect.


### Install gdb

To install [GDB](http://ntraft.com/installing-gdb-on-os-x-mavericks/) type:

`brew tap homebrew/dupes`

`brew install gdb`

#### Certifying GDB
(Steps 1-10 can be done while gdb is installing)

1. Open up the Keychain Access application (/Applications/Utilities/Keychain Access.app). Navigate via the menu to Keychain Access > Certificate Assistant > Create Certificate...
2. Enter a name for the certificate e.g. "gdb-cert".   Set the fields exactly as following:

  ```
    Identity Type: Self Signed Root
    Certificate Type: Code Signing
  ```
3. Check the 'Let me override defaults' box.
4. Click 'Continue'
5. In the 'Certificate Information' window, enter '6666' in the 'Validity Period (days)' box (to make sure the certificate never runs out.
6. Click 'Continue' and keep clicking 'Continue' until you come to the 'Specify a Location For the Certificate' window.
7. Select 'System' from the 'Keychain' dropdown list.
8. Click 'Create' - the certificate should now have been created.
9. You now need to make sure that the certificate is always trusted. Select 'System' on the left hand side and in the list of certificates, right-click on the gdb-cert certificate and select 'Get Info'. Under the 'Trust' section, set 'Code Signing' to 'Always Trust'.
Now that we have a certificate, we need to use it to sign gdb.
10. Quit Keychain access. You **must** do this before doing anything else.
11. To make sure that the new certificate is picked up you need to kill the taskgated process. You can do this in the terminal by doing the following:

  ```
  ps -e | grep taskgated

  56822 ??         0:03.11 /usr/libexec/taskgated -s
  60944 ttys002    0:00.00 grep --color=auto taskgated
  ```
  The first number in the above output is the PID. Use this to kill the process (it will immediately restart itself).

  ```
  sudo kill -9 56822
  ```
  (restarting your computer negates the need for you to kill the taskgated process)

12. Now you can finally code sign gdb.

  ```
  codesign -s gdb-cert $(which gdb)
  ```


Now you should be all set! The OS X Keychain may ask for your password the first time you attempt to debug a program, but it should work!



###Install Alternative text editors
Sublime can be left on the machines for the students to trial but we'd be breaking the licence if we used it continuously.  Atom and Visual Studio Code are free alternatives

Download and install [Atom](https://atom.io/)


###Install JDK with Netbeans

Download and install [JDK with Netbeans](http://www.oracle.com/technetwork/java/javase/downloads/jdk-netbeans-jsp-142931.html)

###Android SDK and Android Studio

Download and install [Android Studio](http://developer.android.com/sdk/index.html)

If JDK is successfully installed then Android Studio should install fine.
Alternatively (if you get errors about Platform Tools) then follow [these instructions](https://github.com/Homebrew/homebrew/issues/41702) before reinstalling Studio.


###Virtual Box and Oracle

Download and install [VirtualBox platform for OS X and the Oracle extension](https://www.virtualbox.org/wiki/Downloads)


###Eclipse IDE

Download and install [Eclipse IDE for Java](https://eclipse.org/downloads/)


###UML Diagram software

Download and install [Visual Paradigm Community edition](https://www.visual-paradigm.com/download/community.jsp)
Follow the instructions for [integrating VP with Netbeans](https://www.visual-paradigm.com/tutorials/modelinginnetbeans.jsp)



### Cleanup

Set all new software to the Dock and remove items that will not be needed



If you are setting up a student machine then it is wise to do the following before passing it on:

1. Sign out of iTunes
2. If you had logged into Github for any reason, sign out.
3. Delete all browser history and web data (passwords etc).
4. Delete all downloaded install files (Sublime, PostgreSQL, HipChat, Chrome etc).






