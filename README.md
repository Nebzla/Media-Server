# Media Server Template

The purpose of this repository is to provide a docker compose template that can be cloned onto any device to setup a local media server

**DISCLAIMER:** This service should be used soley for accessing _public domain_ media. Using this to access copyrighted material is a crime, I am not responsible for how you choose to use it.

## Dependencies:

- An active Debrid subscription for use with RDT client to be used for downloading the media to your local device.
- A VPN provider that you can route traffic through, while this isn't necessary with Debrid when the torrents have already being sourced, in order to request torrents from the various sites you require a VPN due to most ISP's blocking such sites.

- Docker & Docker Compose

## Current Services:

- [Prowlarr](https://github.com/Prowlarr/Prowlarr): Provides indexing of various sites for Sonarr and Radarr
- [Sonarr](https://github.com/Sonarr/Sonarr): Finds TV Shows from various indexers
- [Radarr](https://github.com/Radarr/Radarr): Finds Movies from various indexers
- [Flaresolverr](https://github.com/FlareSolverr/FlareSolverr): Allows you to bypass Ckoudfare protection some websites may have

- [RDT Client](https://github.com/rogerfar/rdt-client): Downloads the sourced media from Debrid's servers allowing for quick downloaded provided it is already cached in their servers
- [Jellyfin](https://github.com/jellyfin/jellyfin): An application providing an interace to access and play downloaded media
- [Jellyseerr](https://github.com/Fallenbagel/jellyseerr): An application that allows you to interact with Sonarr and Radarr within one application to source media
- [Gluetun](https://github.com/qdm12/gluetun): Allows traffic to be routed through supported VPNs

# Configuration:

In order to set the server up, `.env` must first be configured to your needs. It should be located in the same directory as your `compose.yaml`

`MEDIADIR`: The directory on your machine in which the media will be saved to

`STACKDIR`: The directory in which the media stack will be located in, this can be the same directory as the `compose.yaml` if desired

`PUID`: Your user ID

`GUID`: Your group ID

`TZ`: Your timezone, Region/City

`VPNPROVIDER`: The name of your VPN provider, for example surfshark

`VPNCOUNTRIES`: The country of the VPN location you want to connect to

`WGPRIVATE`: The private Wireguard key aquired from your VPN provider

`WGPUBLIC`: The public Wireguard key aquired from your VPN provider

`WGADDRESS`: The Wireguard address aquired from yoru VPN provider

When these have been setup, you can then boot up the containers with `docker compose up -d`, to shut them down, use `docker compose down`
If everything else has been setup correctly, you should now be able to access webpages on the exposed ports of each service on your local machine

_To be continued_
