# Media Server Template

The purpose of this repository is to provide a docker compose template that can be cloned onto any device to setup a local media server


**DISCLAIMER This service should be used soley for accessing *public domain* media, using this to access copyrighted material is a crime.**


## Dependencies:
- An active Debrid subscription for use with RDT client to be used for downloading the media to your local device.
- A VPN provider that you can route traffic through, while this isn't necessary with Debrid when the torrents have already being sourced, in order to request torrents from the various sites you require a VPN due to most ISP's blocking such sites.

- Docker & Docker Compose


## Current Services:
- Prowlarr: Provides indexing of various sites for Sonarr and Radarr
- Sonarr: Finds TV Shows from various indexers
- Radarr: Finds Movies from various indexers
- Flaresolverr: Allows you to bypass cloudfare protection some websites may have

- RDT Client: Downloads the sourced media from Debrid's servers allowing for quick downloaded provided it is already cached in their servers
- Jellyfin: An application providing an interace to access and play downloaded media
- Jellyseerr: An application that allows you to interact with Sonarr and Radarr within one application to source media
- Gluetun: Allows traffic to be routed through supported VPNs

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

*To be continued*