# godot-game-client
godot-game-client

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
