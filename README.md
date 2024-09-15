# godot-game-client
Godot Game Client allows users to download and play multiple Godot applications using a single application. The client works on Desktop, Mobile, and Web using the OpenGL compatability mode.

## Working with Packs
This "Client" can import Packs (`.pck`) from remote resources. They are cataloged in [scenes/main-menu/recommendations.json](scenes/main-menu/recommendations.json). To avoid file name conflicts with the Client and other packs, the Pack's scenes should be in a sub-folder with the Pack's name. E.g., `res://scenes/my_pack`.

### Setup Remote Repo
1. An new, empty file named `ci/.gdignore`
	1. Prevents Godot from importing files contained in this folder
1. A new file named `ci/export-pack.ps1`
	1. Paste in the contents of [export-pack.ps1](ci/export-pack.ps1)
1. A new file named `ci/thumbnail.png`
	1. Use the [placeholder](ci/286x160.png)
1. Run the script and commit
	The pack and thumbnail should be publically available

### Add Remote Repo to Catalog
1. Open/Edit [scenes/main-menu/recommendations.json](scenes/main-menu/recommendations.json)
1. Add a new JSON Object with the following:
	- "title": Displayed under the thumbnail
	- "short": A brief description, displayed under the title
	- "long": A longer description, displayed on the details page
	- "thumb": The displayed image for the project in the catalog
	- "pack": The name of the pack, when it's imported
		- E.g., `res://scenes/my-pack` -> `my-pack`
	- "scene": The main scene (entry-point) of the pack
	- "url": The pack download URL
1. Once the Client loads, it will attempt to download the Thumbnail
	- If the user clicks the thumbnail, the "pack" will download
	- Once the download completes, the "scene" will be loaded

---

## Run Application on Remote Devices Using Wifi
Godot Remote Debug is great for testing on your `localhost`. This section will enable testing using devices on the same wifi network.

### Install and Enable Live Server
[Live Server](https://marketplace.visualstudio.com/items?itemName=ritwickdey.LiveServer) allows you to host web pages, locally, from VSCode.

### Generate HTTPS Certificate
"Secure Context - Check web server configuration (use HTTPS)" The following features required to run Godot projects on the Web. Do the following to setup
1. Download and install the [ssl binary](https://wiki.openssl.org/index.php/Binaries)
	- I use [OpenSSL for Windows](https://slproweb.com/products/Win32OpenSSL.html)
	- Confirm installation by running `openssl -v` in cmd/terminal
1. Open the root folder using [VS Code](https://code.visualstudio.com/)
    - If you use GitHub Desktop, select the "Open in Visual Studio" button
1. Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal)
1. Run `openssl genrsa -aes256 -out localhost.key 2048`
	- You will be prompted for a "PEM pass phrase", remember this for the next step
	- `godot`
1. Run `openssl req -days 3650 -new -newkey rsa:2048 -key localhost.key -x509 -out localhost.pem`
	- You will be prompted for the "PEM pass phrase"
	- Fill out the rest of the information as the prompts request
		- "Country Name (2 letter code) [AU]:"`US`
		- "State or Province Name (full name) [Some-State]:"`WA`
		- "Locality Name (eg, city) []:"`Seattle`
		- "Organization Name (eg, company) [Internet Widgits Pty Ltd]:"`Timothy Cope`
		- "Organizational Unit Name (eg, section) []:"`Development`
		- "Common Name (e.g. server FQDN or YOUR name) []:"`localhost`
		- "Email Address []:"`kirbycope@gmail.com`
1. Open/Create `.vscode/settings.json` in the root of your project
1. Copy+paste the following:
	```
	{
		"liveServer.settings.root": "/",
		"liveServer.settings.https": {
			"enable": true,
			"cert": "localhost.pem",
			"key": "localhost.key",
			"passphrase": "{PEM pass phrase}"
		}
	}
	```
	- Replace `{PEM pass phrase}` with your "PEM pass phrase" from earlier
1. Restart VSCode (or the terminal, at least)

### Running/Hosting the App Locally
1. In VSCode's Explorer right-click on [docs/index.html](docs/index.html) and select "Open with Live Server"
1. When you visit [https://127.0.0.1:5500/docs/index.html](https://127.0.0.1:5500/docs/index.html)
1. To get your "Host Local IP Address", use terminal to run:
	- [Windows] `ipconfig`
	- [MacOS] `ipconfig getifaddr en0`
1. On a device connected to the same wifi as the host, navigate to `https://{host.local.ip.address}:5500/docs/index.html`
	- Replace `{host.local.ip.address}` with your "Host Local IP Address" from earlier
