# Submersion Flatpak

Flatpak packaging of [Submersion](https://github.com/submersion-app/submersion). The application manifest is [io.github.submersion.yml](io.github.submersion.yml).

The manifest downloads the upstream x86_64 release bundle and preserves its executable, `lib/`, and `data/` layout under `/app/lib/submersion`. Runtime is `GNOME Platform 51`, which provides GTK3, WebKitGTK 4.1, libsoup 3, libsecret, and the other system libraries. 

The package builds `ffmpegthumbnailer` 2.3.1 from a pinned source commit and installs it in `/app/bin`. Submersion finds it through `PATH` to generate PNG video posters, using the runtime's FFmpeg libraries.

## Build and install

Install Flatpak and flatpak-builder using your distribution's package manager, then add Flathub and build:

```sh
flatpak remote-add --user --if-not-exists flathub \
  https://dl.flathub.org/repo/flathub.flatpakrepo
flatpak-builder --user --install-deps-from=flathub --assumeyes \
  --sandbox --force-clean --repo=repo --install \
  build-dir io.github.submersion.yml
flatpak run io.github.submersion
```

To create a portable installer after building:

```sh
flatpak build-bundle repo Submersion-x86_64.flatpak \
  io.github.submersion --runtime-repo=https://dl.flathub.org/repo/flathub.flatpakrepo
flatpak install --user ./Submersion-x86_64.flatpak
```

## Permissions and data

| Permission | Purpose |
| --- | --- |
| Wayland, fallback X11, IPC | Flutter windows and graphics |
| All devices | GPU rendering and the native dive-computer implementation's direct HID/serial access |
| Network | Maps, cloud synchronization, OAuth, and online information |
| PulseAudio | WebKit audio |
| CUPS | Printing |
| `--persist=Submersion` | Private persistent database and media directory |
| Secret Service | Secure credential storage |
| Notifications | Desktop notifications |
| NetworkManager | Network connectivity detection |
| BlueZ | Bluetooth dive-computer discovery and communication |
