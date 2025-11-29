# RyzenProfile
## runit/systemd supervisor and daemon that runs ryzenadj exactly once per change: kills old worker, spawns new one on power source or profile switch

## Features?
- Applies profiles on AC\<-\>BAT switch or manual profile change.
- Auto-detects runit and systemd at runtime.
- Generates profiles (38-100% of stock limits).
- Shows active workers, kills stale one.
- No external dependencies beyond `ryzenadj` and `bash>=4.8`.
- Profiles live in `/etc/ryzenprofile/profiles/*` (just plain text).

## Quick start
```bash
ryzenprofile --init --install   # done (profiles generated + daemon installed + service enabled (balanced))
```

## Quick commands
```bash
ryzenprofile --list             # list available profiles
ryzenprofile --status           # show active workers
ryzenprofile --kill-stale       # kill stale workers
ryzenprofile -s -p powersave    # set powersave profile
```

## Custom args for profile
```bash
sudo ryzenprofile -s -p powersave --custom-args="--power-saving --gfx-clk=1400"    # Set custom args for `powersave`
sudo ryzenprofile -s --custom-args=""                                              # Clear custom args for current profile
```

## Uninstall
```bash
ryzenprofile -u
```

## Notes
- Profiles are generated **once** at install time from your laptop’s real OEM limits.  
  -> They will stay correct even after BIOS updates or if you move the config to another machine with the same CPU family.
- The daemon runs as root (ryzenadj needs it). Workers are one-shot sh processes, never bash loops.
- If you ever want completely custom limits: just copy one of the generated profiles to `/etc/ryzenprofile/profiles/balanced` and edit the numbers. Then `ryzenprofile -s -p my_custom_profile`.
- Works on every Ryzen mobile APU. If something looks wrong, run `sudo rm /etc/ryzenprofile/stock_limits` and `ryzenprofile --generate-profiles` again – it will refresh the cached OEM limits and generate 9 profiles.

## LICENSE: MIT

