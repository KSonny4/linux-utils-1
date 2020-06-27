# linux-utils
## Description
Various utilities and settings for the Linux system, Fedora in particular.

I have this repository cloned into my `$HOME` directory and the files symlinked to their respective locations. That is,
* `.zshrc` goes to `$HOME/.zshrc`;
* `aliases.zsh` goes to `$HOME/.oh-my-zsh/custom/aliases.zsh`;
* `init.vim` goes to `$HOME/.config/nvim/init.vim`;
* `kitty.conf` goes to `$HOME/.config/kitty/kitty.conf`;
* `user-overrides.js` goes to `$HOME/.mozilla/firefox/<profile-directory>` (see the Configuration/Firefox section below).


## Software
When doing a fresh installation, first update:
* Packages: `sudo dnf distro-sync -y && sync`; the `sync` command synchronized cached writes to persistent storage.
* Firmware: `sudo fwupdmgr refresh && sudo fwupdmgr update --verbose`.
* Flatpaks: `flatpak update && sudo flatpak update`.

The following software should then be installed.

### Internet
* Firefox
* Thunderbird
    * [Thunderbird and Gmail](https://support.mozilla.org/en-US/kb/thunderbird-and-gmail)
    * [IMAP settings for Gmail](https://support.google.com/mail/answer/78892?hl=en)
* Transmission

### Development
* [Anaconda](https://www.anaconda.com/distribution/)
    * create a `base` environment, enable it
    * autoflake
    * black
    * flake8
    * mypy
    * `conda install -c numba icc_rt` -- install Intel SVML to speedup Numba code.
* [Lua](https://www.lua.org/)
* Git
* [Neovim](https://neovim.io/)
    * [vim-plug](https://github.com/junegunn/vim-plug)
* Make

### System utilities
* gnome-tweaks & No Topleft Hot Corner to disable the annoying "feature" when activities are opened whenever the cursor hits the top-left corner.
* Dash to Panel
* [kitty](https://sw.kovidgoyal.net/kitty/)
    * Along with the steps specified on the linked page, it is necessary to perform `sudo ln -s ~/.local/kitty.app/bin/kitty /usr/bin/kitty`.
* zsh
    * [oh-my-zsh](https://ohmyz.sh/)
    * [firacode](https://github.com/tonsky/FiraCode/wiki/Linux-instructions)
    * [agnoster theme](https://github.com/agnoster/agnoster-zsh-theme) and [powerline fonts](https://github.com/powerline/fonts)
    * [zsh-autosuggestions](https://github.com/zsh-users/zsh-autosuggestions/blob/master/INSTALL.md#oh-my-zsh)
    * [zsh-syntax-highlighting](https://github.com/zsh-users/zsh-syntax-highlighting/blob/master/INSTALL.md)
    * set as the default shell: `chsh -s $(which zsh)`
* [fzf](https://github.com/junegunn/fzf)
* [ag](https://github.com/ggreer/the_silver_searcher)
* htop
* sensors
    * `sudo dnf install lm_sensors -y && sudo sensors-detect --auto && sudo watch sensors`
* [pass](https://www.passwordstore.org)
    * [pass-extension-tail](https://github.com/palortoff/pass-extension-tail)
    * The autocompletion is in `.zshrc`. If it does not work, do `rm $HOME/.zcompdump*` and restart zsh using `exec zsh`.

### Media
* [VLC](https://www.videolan.org/vlc/download-fedora.html)
    * Comes with many codecs necessary to play certain online videos.
    * Rebind `Left/Right` from `Navigate left/right` to `Very short backwards/forward jump`.
* [cmus](https://cmus.github.io/)

### Communication
* Discord
    * Requires some non-standard RPMs but should be possible to install at this point due to addition of some RPMs when installing VLC.
* Telegram


## System configuration (not exhaustive)
* Turn off bluetooth
* Reset the default root password
* Enable SSD trimming
    ```
    systemctl is-enabled fstrim.timer  # Check
    systemctl enable fstrim.timer  # Enable
    systemctl is-enabled fstrim.timer  # Check again
    ```
* [Hosts file](https://github.com/StevenBlack/hosts/)
    * Unified hosts + fakenews + gambling
* Nautilus
    * Sort directories before files
* Fix the retarded `<Alt>Tab` and `<Shift><Alt>Tab` behavior.
    * Install `dconf-editor`, go to `org/gnome/desktop/wm/keybindings`.
    * Move the values from `switch-applications` to `switch-windows`.
    * Move the values from `switch-applications-backward` to `switch-windows-backward`.
* Set firacode as the system monospaced font in system configuration.
* Set ``<Ctrl>` `` as a keyboard shortcut to launch kitty.
* [Backup LUKS headers](https://fedoraproject.org/wiki/Disk_Encryption_User_Guide#Backup_LUKS_headers)


## Firefox configuration
Note that this is a privacy/security oriented configuration. After the initial setting, there will be a period when you have to tweak the addons to your taste so that they do not block websites you visit. For instance, embedded YouTube videos or gifs will not work until you enable them in uMatrix. To do this correctly requires care, otherwise you end up breaking something or losing some degree of privacy.

If in doubt, do not use the uMatrix addon. Along with Skip Redirect, it is expected to cause the most trouble. The other addons should be fairly harmless regarding website breakage.

### Post-installation cleanup
* Create a new profile. Importing data from an old profile is addressed towards the end of this section.
* Remove system addons
    * Either `cd /usr/lib/firefox/browser/features` or `cd /usr/lib64/firefox/browser/features` followed by `sudo rm *.xpi`
    * Note that this must be redone every time Firefox is updated

### Addons
1. [Canvas blocker](https://addons.mozilla.org/en-US/firefox/addon/canvasblocker/)
    * **General**
        * `Expert mode: enabled`
        * `Block mode: fake`
        * `Faking`
            * `Random number generator: non-persistent`
    * **APIs**
        * `Canvas API`
            * `Protected part of the canvas API: readout`
            * `Protected API features:` all
        * `Audio API`
            * `Protected audio API: enabled`
            * `Protected API features:` all
        * `History API`
            * `Protected API features:` all
        * `Window API`
            * `Protected window API: enabled`
            * `Protected API features:` all
        * `DOMRect API`
            * `Protected DOMRect API: enabled`
            * `Protected API features:` all
    * **Misc**
        * `Block data URL pages: disabled`
2. [Clear URLs](https://addons.mozilla.org/en-US/firefox/addon/clearurls/)
    * `Prevent tracking injection over history API: enabled`
3. [CSS Exfil Protection](https://addons.mozilla.org/en-US/firefox/addon/css-exfil-protection/)
4. [Decentraleyes](https://addons.mozilla.org/en-US/firefox/addon/decentraleyes/)
    * Enable everything except `Block requests for missing resources`
5. [Smart HTTPS](https://addons.mozilla.org/en-US/firefox/addon/smart-https-revived/)
6. [Skip Redirect](https://addons.mozilla.org/en-US/firefox/addon/skip-redirect/)
7. [Facebook Container](https://addons.mozilla.org/en-US/firefox/addon/facebook-container/)
8. [Firefox Multi-Account Containers](https://addons.mozilla.org/en-US/firefox/addon/multi-account-containers/)
    * Create desired containers, visit all matching sites in these containers and check `Always open in this container`
    * Then (if you wish) visit them again and check `Remember my decision` to always open them in the assigned container
9. [Temporary Containers](https://addons.mozilla.org/en-US/firefox/addon/temporary-containers/)
    * **General**
        * `Automatic mode: enabled`
    * **Isolation**
        * `Middle Mouse: Different from Tab Domain & Subdomains`
        * `Ctrl/Cmd + Left Mouse: Different from Tab Domain & Subdomains`
        * `Left Mouse: Different from Tab Domain & Subdomains`
10. [uBlock Origin](https://addons.mozilla.org/en-US/firefox/addon/ublock-origin/)
    * **Settings**
        * `Hide placeholders of blocked elements: enabled`
        * `I am an advanced user: enabled` -> set `suspendTabsUntilReady` to `true` -> `I am an advanced user: disabled`
        * **Privacy**
            * Enable all
        * **Default behavior**
            * `Disable cosmetic filtering: disabled`
    * **Filter lists** -- check the following
        * `Auto-update filter lists`
        * `Parse and enforce cosmetic filters`
        * `My filters`
        * `Built-in:` all except `uBlock filters - Experimental`
        * `Ads`
            * `Adblock Warning Removal List`
            * `EasyList`
        * `Privacy:` all
        * `Malware domains:` all
        * `Annoyances`
            * `AdGuard Annoyances`
            * `EasyList Cookie`
            * `Fanboy's Annoyance List`
            * `uBlock filters - Annoyances`
        * `Multipurpose`
            * `Dan Pollock's hosts file`
            * `Peter Lowe's Ad and tracking server list`
        * `Regions, languages`
            * `CZE, SVK: EasyList Czech and Slovak`
            * `RUS: AdGuard Russian`
            * `RUS: RU AdList`
        * `Custom:` [Import this file for cryptominer blocking](https://raw.githubusercontent.com/hoshsadiq/adblock-nocoin-list/master/nocoin.txt) -- simply paste the link (not the file contents) to the 'custom' field.
11. [uMatrix](https://addons.mozilla.org/en-US/firefox/addon/umatrix/)
    * Click the toolbar icon and then the title bar to get to the settings dashboard
    * **Settings**
        * **Convenience**
            * `Show the number of blocked resources on the icon: enabled`
            * `Hide the placeholder of blacklisted elements: enabled`
            * `Spoof <script> tags when 1-st party scripts are blocked`
        * **Privacy**
            * `Delete blocked cookies: enabled`
            * `Delete non-blocked session cookies 60 minutes after the last time they have been used`
            * `Delete local storage content set by blocked hostnames: enabled`
            * `Clear browser cache every 60 minutes`
            * `Spoof HTTP referrer string of third-party requests: enabled`
            * `Block all hyperlink auditing attempts: enabled`
    * **My rules**
        * Add `no-workers: * true`, save and commit (disables web workers)
        * Add [these rules](https://git.synz.io/Synzvato/decentraleyes/wikis/Frequently-Asked-Questions) since Decentraleyes is used as well
    * **Assets**
        * `Auto-update assets: enabled`
        * Disable all hosts files filter lists, purge caches and save (we use uBlock Origin to control the static filters)
        * `Ruleset recipes for English websites: enabled` (a puzzle piece icon will appear on the uMatrix panel allowing to quickly import a community-created ruleset)
    * Lastly, open uMatrix on an arbitrary page, click the star symbol at the top to set the global scope (!) and disable all 1st party requests (by resetting `1st-party` on the left). While in the global scope, selectively enable CSS and images for all sites (by setting `css` and `image` on the top), and 1st party cookies and frames (by clicking their respective fields inside the matrix).
12. [Vimium-FF](https://addons.mozilla.org/en-US/firefox/addon/vimium-ff/)
    * Add the following two rules to the blacklist. They are to disable vimium-ff in Jupyter notebooks, typically found on such URLs.
        * `http://localhost*`;
        * `https://localhost*`;
        * `http://\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3}*`;
        * `https://\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3}*`.

### Configuration -- preferences
* This section concerns the `about:preferences` part of the options. Some of it may be overriden by the `user.js` file set below, but there are parts that the file does not address. These are mainly convenience and user interface settings.
* First, click the main menu -> `Customize`. Then, clear the top bar from all the addon icons and mess that Firefox comes with. It should be enough to leave only uBlock Origin, uMatrix and Firefox Multi-Account Containers.
* Also select compact mode and dark theme.

1. **General**

**Startup**
* `Restore previous session: enabled`

**Tabs**
* `Ctrl+Tab cycles through tabs in recently used order: disabled`

**Language**
* `Check your spelling as you type: disabled`

**Files and Applications**
* `Always ask you where to save files`

**Browsing**
* `Use autoscrolling: disabled`
* `Use smooth scrolling: enabled`
* `Show a touch keyboard when necessary: disabled`
* `Recommend extensions as you browse: disabled`
* `Recommend features as you browse: disabled`

2. **Home**

**Firefox Home Content**
* Disable all

3. **Search**
* `Default Search Engine: DuckDuckGo`
* `Provide search suggestions: disabled`
* `One-Click Search Engines | Keywords`
    * `DuckDuckGo | d`
    * `Wikipedia (en) | w`
* Optionally add `YouTube | y` and `Metal Archives (band name search) | m`

4. **Privacy & Security**

**Content blocking**
* `Enhanced Tracking protection: Custom`
    * `Cookies: enabled, All third-party cookies`
    * `Tracking content: enabled, In all windows`
    * `Cryptominers: enabled`
    * `Fingerprinters: enabled`
* `Send Do Not Track: Only when Firefox is set to block known trackers`

**Logins and Passwords**
* `Ask to save logins and passwords for websites: disabled`
* `Saved logins` -> `Autofill logins and passwords: disabled`

**History**
* Firefox will `Use custom settings for history`
* `Always use private browsing mode: disabled`
    * `Remember browsing and download history: enabled`
    * `Remember search and form history: disabled`

**Address bar**
* `Browsing history: enabled`
* `Bookmarks: enabled`
* `Open tabs: enabled`

**Permissions**
* `Location: Settings` -> `Block new requests asking to access your location`
* `Camera: Settings` -> `Block new requests asking to access your camera`
* `Autoplay: Settings` -> `Block Audio and Video`
* `Virtual Reality: Settings` -> `Block new requests asking to access your virtual reality devices`
* `Block pop-up windows: enabled`
* `Warn you when websites try to install add-ons: enabled`
    * Clear exceptions
* `Prevent accessibility services from accessing your browser: enabled` (requires Firefox restart)

**Firefox Data Collection and Use**
* Disable all

**Security**
* Enable all

**Certificates**
* `Ask you every time`

### Configuration -- user.js
* We use [ghacks-user.js](https://github.com/ghacksuserjs/ghacks-user.js)
* Once the `user-overrides.js` file from this repository is symlinked to the Firefox profile directory, run the `updater.sh` [script](https://raw.githubusercontent.com/ghacksuserjs/ghacks-user.js/master/updater.sh) within the same directory.

### Import data
* Consult [this document](https://support.mozilla.org/en-US/kb/profiles-where-firefox-stores-user-data) where each profile element can be found.
* I transfer *Bookmarks, Downloads and Browsing History* (files `places.sqlite`, `bookmarkbackups` and `favicons.sqlite`) and *Stored session* (file `sessionstore.jsonlz4`).


### Cleanup
* Finally, after having fully configured Firefox, press `Ctrl+Shift+Del` and clear everything except `Browsing & Download History`.
