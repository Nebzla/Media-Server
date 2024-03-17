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

# Docker Setup:

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

Before starting the containers, we first must create a new docker network named media, to do so, use `docker network create media`

After this, you can then boot up the containers with `docker compose up -d`, to shut them down, use `docker compose down`
If everything else has been setup correctly, you should now be able to access webpages on the exposed ports of each service on your local machine

# Server Configuration

Now that the servers are running, we must get the servers to communicate with eachother, following these steps should have everything working:

- Open prowler (`localhost:9696` by default), head to Settings => Indexers and add the FlareSolverr proxy, this will allow us to bypass Cloudfare protection for the indexers that have it. Give the proxy a tag such as `flaresolverr`; the host can be left as default.

- Add your desired indexers to prowlarr under the indexers tab, these will be sent across to sonarr and radarr later on. If they have Cloudfare DDOS protection, add the flaresolverr tag

- Open RDT Client on port 6500, and enter your API token for your Debrid provider, for Real Debrid, your API token can be found [here](https://real-debrid.com/apitoken)
Inside of RDT, navigate to Settings => Download Client, and set your download path to be /media. Set your mapped path to be the same as what you entered into your .env file for `MEDIADIR`. Make sure to save at the bottom once done

- Open either Sonarr (8989) or Radarr (7878) and navigate to Settings => Download Clients. From here, add qBittorrent as a client, and change the port to 6500. While we aren't using qBittorrent, RDTClient spoofs itself as it in order to function. Set your username and password to the same one you used to setup RDT client. 
Then, under Settings => Media Management, add /media as a root folder.
Next, head to the General tab while still under settings, and copy the API key. Navigate back to Prowlarr, head to Settings => Apps and add Sonarr/Radarr depending on which one you've configured first, entering the API Key in where asked. 
These steps will then need to be repeated for the other one you haven't yet done.

- At this stage, if all the previous steps have been followed correctly, adding media to either Radarr or Sonarr should sucessfully download it, however as of current, this interface isn't all that user friendly. This is where Jellyfin/Jellyseerr come in. First, open Jellyfin (8096) and follow the setup for an account. You can leave everything else as default, ignoring adding folders for now.

- Once you've created a Jellyfin account, we will then need to link it to Jellyseerr (5055). Select "Use your Jellyfin account" instead, and enter your details. For the Jellyfin URL, you'll have to use the full address: `http://127.0.0.1:8096`

- Now on the second stage, head back to Jellyfin and add a new folder that points to /media, go back to Jellyseerr, and refresh so that it shows up, enable it and continue. We will now, like before enter in the API key for Radarr and Sonarr, using the default port and the localhost IP (127.0.0.1), with the same /media root folder. Configure everything else as you desire. Make sure to enable both as the default setup in order to continue

- Now you should be greeted with an usual steaming service-like homepage. Choose some *public domain* media of your liking, and request it. If it can be found in the indexers you set up in prowlarr, it should download straight to your device if all has gone successfully. You can add new indexers as any time in prowlarr.
