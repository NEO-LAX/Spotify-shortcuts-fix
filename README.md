1. Install AutoKey and Dependencies

    For KDE (Wayland):
    Install autokey-qt from AUR (using an AUR helper like yay):

yay -S autokey-qt

Also, install playerctl and ensure pactl (from the pulseaudio or pipewire-pulse package) is available:

    sudo pacman -S playerctl

2. Create the Scripts in AutoKey
a. Spotify Previous Track

    Open autokey-qt.

    Create a new script and name it Spotify Previous Track.

    Paste the following code:

    import subprocess
    active_class = window.get_active_class()
    if "Spotify" in active_class:
        subprocess.Popen(["playerctl", "--player=spotify", "previous"])
    else:
        keyboard.send_keys("<ctrl>+<left>")

    Assign the hotkey Ctrl+Left.

b. Spotify Next Track

    Create another new script and name it Spotify Next Track.

    Paste the following code:

    import subprocess
    active_class = window.get_active_class()
    if "Spotify" in active_class:
        subprocess.Popen(["playerctl", "--player=spotify", "next"])
    else:
        keyboard.send_keys("<ctrl>+<right>")

    Assign the hotkey Ctrl+Right.

c. Spotify Volume Up

    Create a new script named Spotify Volume Up.

    Paste this code:

    import re
    import subprocess

    def get_spotify_sink_id():
        try:
            output = subprocess.check_output(["pactl", "list", "sink-inputs"]).decode("utf-8")
            blocks = output.split("Sink Input #")[1:]
            for block in blocks:
                sink_id = block.split()[0]
                if re.search(r'application\.name\s*=\s*"?spotify"?', block, re.IGNORECASE):
                    return sink_id
            return None
        except Exception as e:
            print("Error finding sink ID:", e)
            return None

    active_class = window.get_active_class()
    if "Spotify" in active_class:
        sink_id = get_spotify_sink_id()
        if sink_id:
            subprocess.Popen(["pactl", "set-sink-input-volume", sink_id, "+5%"])
        else:
            print("Spotify sink not found")
    else:
        keyboard.send_keys("<XF86AudioRaiseVolume>")

    Assign a suitable hotkey (for example, the XF86AudioRaiseVolume key).

d. Spotify Volume Down

    Create another new script named Spotify Volume Down.

    Paste the following code:

    import re
    import subprocess

    def get_spotify_sink_id():
        try:
            output = subprocess.check_output(["pactl", "list", "sink-inputs"]).decode("utf-8")
            blocks = output.split("Sink Input #")[1:]
            for block in blocks:
                sink_id = block.split()[0]
                if re.search(r'application\.name\s*=\s*"?spotify"?', block, re.IGNORECASE):
                    return sink_id
            return None
        except Exception as e:
            print("Error finding sink ID:", e)
            return None

    active_class = window.get_active_class()
    if "Spotify" in active_class:
        sink_id = get_spotify_sink_id()
        if sink_id:
            subprocess.Popen(["pactl", "set-sink-input-volume", sink_id, "-5%"])
        else:
            print("Spotify sink not found")
    else:
        keyboard.send_keys("<XF86AudioLowerVolume>")

    Assign a suitable hotkey (for example, the XF86AudioLowerVolume key).

3. Ensure AutoKey Starts at Login

    Open System Settings → Startup and Shutdown → Autostart in KDE.
    Add autokey-qt so that it starts automatically when you log in.

4. Test Your Setup

    Open Spotify and make sure its window is active.
    Press the assigned hotkeys (Ctrl+Left, Ctrl+Right, Volume Up, and Volume Down).
    When Spotify is active, the scripts should control Spotify (switch tracks or adjust volume). In other applications, the keys should perform their default actions.
