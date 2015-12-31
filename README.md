[control-pianobar-osx](https://github.com/ubuntuslave/control-pianobar-osx)
================

## Description

This is a fork from [control-pianobar](http://bruce-connor.github.io/control-pianobar/)
that I hacked a little bit in order to make it work with [pianobar](http://6xq.net/projects/pianobar/) and *Growl* in OS X

**UPDATE 12/29/15:** Improved support in El Capitan by porting zenity to the seemingly-better-supported [CocoaDialog](http://mstratman.github.io/cocoadialog/).

The original project can be found at
[http://bruce-connor.github.io/control-pianobar/](http://bruce-connor.github.io/control-pianobar/).
Go there if you require to use this in Linux derivatives!

## Installation:

### Install Pianobar

#### Using *Homebrew*, do

    $ brew install pianobar

##### Some Problems' Troubleshooting
- if the dylib is broken, for instance with p11-kit, just reinstall the lib (brew uninstall p11-kit && brew install p11-kit)
- if `guile` cannot find `GNU libtool`, it might not be linked properly, so run `$ brew unlink libtool && brew link libtool`
  - Then, reinstall `gnutls` as following
   
        $ brew rm gnutls & brew install gnutls $(brew options gnutls | grep -E '^--with-' - | tr '\n' ' ')
          
- Missing header `neaacdec.h`, you need to install the **AAC decoder** that comes in `faad2`, it may not be linked
    
        $ brew unlink faad2 && brew link faad2

- Sometimes, this is an issue (only realized by using --verbose mode), so reinstall the *mad* package:

        $ brew rm mad & brew install mad
          
- Runtime missing library:

        $ brew unlink json-c && brew link json-c                   


#### From source
Getting *pianobar* from [the pianobar project at github](https://github.com/PromyLOPh/pianobar/) and follow their instructions

### Setting up *control-pianobar*

1. Get the two *bash* scripts and put them and pandora.jpg (or any other icon) in your *~/.config/pianobar/* folder
1. Create a symbolic link to `control-painobar.sh` from your `~/bin` directory or other folder in your `PATH` variable

        ln -s ~/.config/pianobar/control-pianobar.sh ~/bin/
    
1. Install Requirements
  1. Install the `growlnotify` command by getting it from [http://growl.info/downloads](http://growl.info/downloads)
  1. Install `wget` from Homebrew and `cocoadialog` from homebrew-cask:
      
        $ brew install wget Caskroom/cask/cocoadialog

  1. Verify path to `cocoadialog` binary in `~/.config/pianobar/control-pianobar.sh`

  1. Make a `pidof` script with
            
    ```
    #!/bin/bash
    ps axc|awk "{if (\$5==\"$1\") print \$1}";
    ```

    1. Make it executable with `$ chmod a+x pidof`
  
    1. Save it somewhere in your `$PATH` locations so it can be found

### Usage

Refer to the [original project's website](http://bruce-connor.github.io/control-pianobar/) for clearer instructions on how to use `control-pianobar` or do key bindings to your keys.

In a nutshell: I use [BetterTouchTool](http://bettertouchtool.net) to keybind the commands (***not working directly with the bash scripts command) so I made individual *AppleScript* applications for each command. For example,

In particular, for the initial play script, this works fine for me:

    tell application "System Events"
        if (count (processes whose bundle identifier is "com.apple.Terminal")) is 0 then
            tell application "/Applications/Utilities/Terminal.app"
                do script "~/.config/pianobar/control-pianobar.sh p;" in window 0
            end tell
        else
            tell application "/Applications/Utilities/Terminal.app"
                do script "~/.config/pianobar/control-pianobar.sh p; exit;"
            end tell
        end if
    end tell

For any other control option, like `next`, the *AppleScript* would like as follows:

    tell application "Terminal"
        do script ("~/.config/pianobar/control-pianobar.sh next; exit;")
    end tell
