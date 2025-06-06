DepotDownloaderMod
===============
IMPROTANT: This Tool require a manifest file to work Due to GetManifestRequestCode Verification.

And I add dump key support: When you are obtaining the game or manifest, the decryption key will be automatically extracted to the "keys.txt" file in the root directory. The format is `{depotId};{keyHex}`

Steam depot downloader utilizing the SteamKit2 library with depot keys support and many other features. Supports .NET 9.0  

Works with keys from SteamTools:  

https://bbs.steamtools.net/forum.php?mod=viewthread&tid=44&highlight=%E5%A4%A7%E5%AE%B6  
https://bbs.steamtools.net/forum.php?mod=viewthread&tid=16&highlight=%E5%A4%A7%E5%AE%B6  

* Update: Added `Scripts\storage_depotdownloadermod.py` to generate a bat file to download with depot keys and manifests from Many depot libraries.

* Update: You can decrypt the latest manifest in [https://youxiou.com/thread-3.htm](https://youxiou.com/thread-3.htm)

### Downloading one or all depots for an app with depot keys and manifest file

```(text)
dotnet DepotDownloader.dll -app <id> -depotkeys <depotkeysfile> [-depot <id> [-manifest <id>]] -manifestfile <manifestfile>
```

For example: `dotnet DepotDownloader.dll -app 730 -depot 731 -manifest 7617088375292372759 -depotkeys steam.keys -apptoken 1234567890123456789 -manifestfile 730_7617088375292372759.manifest`

By default it will use anonymous account ([view which apps are available on it here](https://steamdb.info/sub/17906/)).

To use your account, specify the `-username <username>` parameter. Password will be asked interactively if you do
not use specify the `-password` parameter.

### Downloading a workshop item using pubfile id
```powershell
./DepotDownloader -app <id> -pubfile <id> [-username <username> [-password <password>]]
```

For example: `./DepotDownloadermod -app 730 -pubfile 1885082371`

### Downloading a workshop item using ugc id
```powershell
./DepotDownloader -app <id> -ugc <id> [-username <username> [-password <password>]]
```

For example: `./DepotDownloadermod -app 730 -ugc 770604181014286929`

## Parameters

Parameter               | Description
----------------------- | -----------
`-app <#>`				| the AppID to download.
`-depot <#>`			| the DepotID to download.
`-manifest <id>`		| manifest id of content to download (requires `-depot`, default: current for branch).
`-ugc <#>`				| the UGC ID to download.
`-branch <branchname>`	| download from specified branch if available (default: Public).
`-branchpassword <pass>`| branch password if applicable.
`-all-platforms`		| downloads all platform-specific depots when `-app` is used.
`-os <os>`				| the operating system for which to download the game (windows, macos or linux, default: OS the program is currently running on)
`-osarch <arch>`		| the architecture for which to download the game (32 or 64, default: the host's architecture)
`-all-archs`			| download all architecture-specific depots when `-app` is used.
`-all-languages`		| download all language-specific depots when `-app` is used.
`-language <lang>`		| the language for which to download the game (default: english)
`-lowviolence`			| download low violence depots when `-app` is used.
`-pubfile <#>`			| the PublishedFileId to download. (Will automatically resolve to UGC id)
`-username <user>`		| the username of the account to login to for restricted content.
`-password <pass>`		| the password of the account to login to for restricted content.
`-qr`                   | display a login QR code to be scanned with the Steam mobile app
`-no-mobile`            | prefer entering a 2FA code instead of prompting to accept in the Steam mobile app.
`-remember-password`	| if set, remember the password for subsequent logins of this user. (Use `-username <username> -remember-password` as login credentials)
`-depotkeys <depotkeysfile>`  | a list of depot keys to use ('depotID;hexKey' per line)
`-manifestfile <manifestfile>`| Use Specified Manifest file from Steam.
`-apptoken <apptoken>`        | Use Specified App Access Token
`-packagetoken <packagetoken>`| Use Specified Package Access Token
`-dir <installdir>`     | the directory in which to place downloaded files.
`-filelist <file.txt>`	| the name of a local file that contains a list of files to download (from the manifest). prefix file path with `regex:` if you want to match with regex. each file path should be on their own line.
`-validate`				| Include checksum verification of files already downloaded.
`-manifest-only`		| downloads a human readable manifest for any depots that would be downloaded.
`-cellid <#>`			| the overridden CellID of the content server to download from.
`-max-downloads <#>`	| maximum number of chunks to download concurrently. (default: 8).
`-loginid <#>`			| a unique 32-bit integer Steam LogonID in decimal, required if running multiple instances of DepotDownloader concurrently
 `-use-lancache`         | forces downloads over the local network via a Lancache instance.
 `-debug`                | enable verbose debug logging.
 `-V` or `--version`     | print version and runtime.

## Frequently Asked Questions

### Why am I prompted to enter a 2-factor code every time I run the app?
Your 2-factor code authenticates a Steam session. You need to "remember" your session with `-remember-password` which persists the login key for your Steam session.

### Can I run DepotDownloader while an account is already connected to Steam?
Any connection to Steam will be closed if they share a LoginID. You can specify a different LoginID with `-loginid`.

### Why doesn't my password containing special characters work? Do I have to specify the password on the command line?
If you pass the `-password` parameter with a password that contains special characters, you will need to escape the command appropriately for the shell you are using. You do not have to include the `-password` parameter on the command line as long as you include a `-username`. You will be prompted to enter your password interactively.

### Why am I getting slow download speeds and frequent connection timeouts?
When downloading old builds, cache server may not have the chunks readily available which makes downloading slower.
Try increasing `-max-downloads` to saturate the network more.
