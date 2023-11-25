<div align="center">
  <img height=128px src="https://i.tycrek.dev/Phoenix-icon-128px.png">

  # *Phoenix Media*
  An all-in-one package to jumpstart your pirate adventure.
</div>

## What's included?

- **[Prowlarr](https://prowlarr.com/)** is used to connect to various indexers, such as 1337x.
- **[Radarr](https://radarr.video/)** manages your movie collection and automatically adds new releases as they come out.
- **[Sonarr](https://sonarr.tv/)** manages your TV collection, similarly to Radarr.
- **[Bazarr](https://www.bazarr.media/)** manages subtitles for all your media.
- **[qBittorrent](https://www.qbittorrent.org/)** downloads anything requested by Radarr or Sonarr.
- **[Caddy](https://caddyserver.com/)** lets you access everything from the internet.

All services are ran via [**Docker Compose**](https://docs.docker.com/compose/) with containers created by [Hotio](https://hotio.dev/).

### What's *not* included?

You might have noticed the lack of a media server in the list. While Hotio does provide containers for [Plex](https://hotio.dev/containers/plex/) and [Jellyfin](https://hotio.dev/containers/jellyfin/), I use [Emby](https://emby.tv/). I do plan to eventually support all three media servers, once I am able to find a clean way to support this.

### Unique features

Some interesting things included with this stack:

- qBittorrent is using a 3rd-party UI: [VueTorrent](https://github.com/WDaan/VueTorrent)
- Caddy allows password-protected access to your media files via `direct.your-domain.here`
- Services are resource-limited to prevent overloading the host

## Getting started (WIP)

I'm still working on this. For now just clone the repo and see if you can figure it out. At the very least, you will need to:

- Set your timezone in `compose.yml`
- Set your domain name in `Caddyfile`
- Add a bcrypt hash to `Caddyfile` ([generate one here](https://bcrypt.online/))
- Any other edits you wish to make

### Filesystem layout

At some point I'll find a way to make this more dynamic but currently, the files are based off the directory structure I use on my own server. That layout is as follows (on a Unix-based filesystem):

```tree
arr/
├── direct-www/
│   └── index.html
├── docker/
│   ├── bazarr/
│   ├── caddy/
│   ├── compose.override.yml
│   ├── compose.yml
│   ├── prowlarr/
│   ├── qbit/
│   ├── radarr/
│   └── sonarr/
├── movies/
│   └── Movies/
└── tv/
    └── TV_Shows/
```

Important things to note here:

- `arr/` is located at the filesystem root, aka `/arr/`.
- `movies/` and `tv/` are **mount points** for high-capacity HDDs.
- `Movies/` and `TV_Shows/` are directories located at the root of their respective HDD.
- qBittorrent requires a path to exist at `/home/phoenix/qbit`. I will try to resolve this.

## How does it work?

Docker Compose makes it very easy to deploy multiple services at once. It also defines an internal network that the containers use to talk to eachother directly.

### What is `compose.override.yml`?

Long story short, Yaml is a really terrible language but luckily Compose implements [a merge feature](https://docs.docker.com/compose/multiple-compose-files/merge/) that bypasses a glaring limitation of Yaml that I won't get into here.

## "This can't be legal."

What exactly is illegal about providing configuration files for open-source software? Nothing. That said, you are subject to whatever piracy laws exist in your country. Don't be dumb.
