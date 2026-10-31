# Installation

Fab listing: <!-- Fab listing URL after publish -->

1. Purchase MI Bulk Editor on Fab and open the Epic Games Launcher.
2. Go to **Unreal Engine > Library > Fab Library**, find MI Bulk Editor, and click **Install to Engine**. Choose your engine version (UE 5.0–5.8). Fab copies the plugin into `Engine\Plugins\Marketplace\MIBulkEditor` of that engine.
3. **Enable it in each project.** Open your project, go to **Edit > Plugins**, search for "MI Bulk Editor", tick **Enabled**, then restart the editor. Repeat for every project where you want it.
4. Check that **Tools > MI Bulk Editor** appears.

**Installing into one project instead:** copy the plugin folder into your project's `Plugins\` folder (`<Project>\Plugins\MIBulkEditor\`), then enable it in **Edit > Plugins** and restart the editor. Don't install it in the engine and in the project at the same time.

Each package is built for one engine version. Use the package that matches your engine (for example, the 5.0 package only works with UE 5.0).

**Requirements:** Windows 64-bit editor. MI Bulk Editor is an editor-only plugin (one Editor module).

**Uninstall:** disable the plugin in **Edit > Plugins** and restart the editor. Material Instances you already changed and saved keep their values; the plugin does not need to be installed to open them.

## FAQ
**I installed it, but Tools > MI Bulk Editor doesn't appear.**
Enable the plugin in **Edit > Plugins** (search for "MI Bulk Editor"), tick **Enabled**, and restart the editor.

**The Content Browser right-click entries are missing.**
**Edit in MI Bulk Editor** appears when you right-click Material Instance assets. **Open Folder in MI Bulk Editor** appears when you right-click a folder. Both need the plugin to be enabled (see above).
