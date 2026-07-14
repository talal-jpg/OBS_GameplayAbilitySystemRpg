3. Easier method: use Steam's generated ID

After adding your game:

Open Flatpak Steam
Launch Aura once
Close Steam

Then check:

grep -a "Aura" ~/.var/app/com.valvesoftware.Steam/.local/share/Steam/userdata/*/config/shortcuts.vdf

You need the shortcut AppID.

Then:

flatpak run com.valvesoftware.Steam steam://rungameid/ID_HERE

talalqazi1994 second steam id