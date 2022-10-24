# mullvad-tailscale (and Zerotier)

This is a simple script that allows you to run MullvadVPN along with Tailscale or Zerotier. 

The script will connect you to a random available Mullvad server.

The script is named `mtc` as per `Mullvad Tailscale Connect`. The script was inspired by [this gist](https://gist.github.com/1player/e9cadfef833d5eb5a23c30223f560147) although it now has been completely rewritten.

If you are using Zerotier, you can also use this script to make it work along with Mullvad. Follow the [Setup](#setup) section to find out how.

### Features

- [x] See uage and help
- [x] Bring up Tailscale + Mullvad with a random server
- [x] Blacklist countries to avoid connecting
- [x] Only use [RAM only servers](https://mullvad.net/en/blog/2022/8/1/expanding-diskless-infrastructure-to-more-locations-system-transparency-stboot/) (diskless)
- [x] Set custom DNS server for Mullvad
- [x] Bring down Mullvad VPN and remove nftables entries.
- [x] Bring down all (tailscale+mullvad+nftables)
- [x] Update the relay list at stratup
- [x] Only apply nftables configuration and do nothing more

## Requirements

- [Mullvad](https://mullvad.net/download/) must be installed in your system so you have the [mullvad cli](https://mullvad.net/en/help/how-use-mullvad-cli/) command available.

- Install the `nftables` package.

- `tailscale` or `zerotier-one` must be installed and configured with its proper setup.


## Setup

1. Clone this repo:

```bash
git clone https://github.com/r3nor/mullvad-tailscale
```

> Or download and extract the latest release source from [releases page](https://github.com/r3nor/mullvad-tailscale/releases).

2. Go to cloned dir: `cd mullvad-tailscale`

3. Set execution permissions on script:

```bash
chmod 700 mtc
```

4. Inspect and **edit** the script file (`mtc`):

- Change the `RULES_DIR` variable to point to the folder where the `mullvad.rules` file from this repository is located. If you cloned the repo, it should be inside the `mullvad-tailscale` folder.

- Modify the `EXCLUDED_COUNTRY_CODES` if you want to exclude any countries from the VPN connection (don't connect to these countries). If you do not want to exclude any CC set this variable to `''`. If you want to add more, just follow the pattern.

5. Edit the `mullvad.rules` file:

- Set your Tailscale/Zerotier network IPs in the `EXCLUDED_IPS` variable (you can use CDIR notation). 
- Set your Tailscale/Zerotier network IPv6 IPs in the `EXCLUDED_IPV6` variable (you can use CDIR notation), leave it blank if there are no IPv6s. 
- Set your Tailscale/Zerotier DNS resolver in `RESOLVER_ADDRS` (Should be `100.100.100.100` for Tailscale).

> You can find the Tailscale/Zerotier IPs in your dashboard. Just copy and paste for each of your devices.

> Note: If you are using Zerotier, the DNS resolver can be found in the `/etc/resolv.conf` file after running `zerotier-one` service. You will find it in a new line, it should look something like `10.X.X.X`.

6. Setup your Mullvad account if you haven't done it yet:

```bash
mullvad account set 1234123412341234
```

## Usage

For Zerotier users, only the `mtc conf` command will work. After running the `mtc conf` command, you can then manually start Mullvad. It will also work if Mullvad is already running. This will change in future releases, as I will try to integrate it with both `Zerotier` and `Tailscale`.

> You must be inside the directory where the script is located, or use it with the absolute path to it. If you want to run the command without specifying the folder where it is located, add the script directory to your PATH variable.

  - To see usage help: `bash mtc`
  
  - To start mullvad+tailscale: `bash mtc up`
    - See `up` help: `bash mtc up -h`
    - To start mullvad+tailscale with RAM servers only: `bash mtc up -r`
    - To set a custom DNS for Mullvad: `bash mtc up -d 1.1.1.1`
    - You can apply all options at once: `bash mtc up -r -d 8.8.8.8`
    
  - To stop mullvad (not tailscale): `bash mtc down`
    - See `down` help: `bash mtc down -h`
    - To stop all (tailscale included): `bash mtc down -a`
    
  - To apply the `nftables` configuration so Mullvad and Tailscale can work and nothing more: `bash mtc conf`
    - To see `conf` help: `bash mtc conf -h`
    - To remove the applied configuration: `bash mtc conf -d
