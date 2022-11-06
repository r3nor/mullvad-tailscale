# Mullvad with Tailscale / Zerotier
<p align="center"><b>Mullvad Nftables script</b></p>

> Note: The script was formerly called `mtc`. It is now `mnf` and has been improved.

This is a simple bash script that will allow you to run Mullvad VPN along with Tailscale or Zerotier on Linux. 

`mnf` (for `Mullvad nftables`) was inspired by [this gist](https://gist.github.com/1player/e9cadfef833d5eb5a23c30223f560147), although it has been completely rewritten and improved.

This script works with Zerotier and Tailscale.

## Features
- [x] Bring up Tailscale/Zerotier and Mullvad with a random server
- [x] Connect to a specific country
- [x] Blacklist countries to avoid connecting to them
- [x] Use only [RAM-only (diskless) servers](https://mullvad.net/en/blog/2022/8/1/expanding-diskless-infrastructure-to-more-locations-system-transparency-stboot/)
- [x] Set custom DNS server(s) for Mullvad
- [x] Bring down all (tailscale/zerotier+mullvad+nftables) or each one selectively
- [x] Automatically update the Mullvad relay list at startup
- [x] and more.

## Requirements

- [Mullvad](https://mullvad.net/download/) must be installed on your system, so that the [mullvad cli](https://mullvad.net/en/help/how-use-mullvad-cli/) command is available.
- Install the `nftables` package.
- `tailscale` or `zerotier-one` must be installed and configured properly.

## Setup
1. Clone this repo:
```bash
git clone https://github.com/r3nor/mullvad-tailscale
```
> Or download and extract the latest release source from [releases page](https://github.com/r3nor/mullvad-tailscale/releases).

2. Go to the cloned dir: `cd mullvad-tailscale`
3. Make the script executable:
```bash
chmod +x mnf
```
4. Inspect and **edit** the script file (`mnf`):
- Change the `RULES_DIR` variable to point to the directory in which the `mullvad.rules` file from this repository is located. If you cloned the repo, it should be inside the `mullvad-tailscale` folder.
- Modify the `EXCLUDE_COUNTRY_CODES` variable if you want to exclude any countries from the VPN connection (don't connect to these countries). If you do not want to exclude any country, set this variable to `''`. If you want to add more, just add most two-letter country codes, separated by spaces.
- Uncomment the `INCLUDE_COUNTRY_CODES` variable if you want to force the connection to specific countries (only connect to these countries). This will override `EXCLUDE_COUNTRY_CODES`. If you want to add more, just add most two-letter country codes, separated by spaces.
5. Edit the `mullvad.rules` file:
- Set your Tailscale/Zerotier network IPs in the `EXCLUDED_IPS` variable (you can use CDIR notation). 
- Set your Tailscale/Zerotier network IPv6 IPs in the `EXCLUDED_IPV6` variable (you can use CDIR notation). If you do not want IPv6 support, comment this line as well as the one starting with `ip6 daddr $EXCLUDED_IPV6` .
- Set your Tailscale/Zerotier DNS resolver in `RESOLVER_ADDRS`.
    - It should be `100.100.100.100` for Tailscale.
    - If you are using Zerotier, the DNS resolver IP can be found in the `/etc/resolv.conf` file after running `zerotier-one` service. You will find it in a new line. It should look like `10.X.X.X`.
> You can find the Tailscale/Zerotier IPs in your dashboard. Just use copy and paste for each of your devices.
6. Setup your Mullvad account if you haven't done it yet:
```bash
mullvad account login 1234123412341234
```

## Usage
> You might want to add `mnf` to your PATH.
[Jump to an example usage](#example)

### up
Apply nftables configuration and connect to Mullvad and Tailscale/Zerotier.
``` bash
mnf up [-OPTIONS]:
    -h | --help         Show this help message
    -r | --ram          No-disk/RAM only Mullvad relays (default: all servers)
    -z | --zerotier     Use Zerotier instead of Tailscale
    -d | --dns          Set custom Mullvad DNS server (i.e. -d 1.1.1.1 or -d 8.8.8.8,1.1.1.1)
    -c | --country      Specify country code(s) to connect to (i.e. -c gb or -c fr,pt,es)
    -f | --file         Specify a particular NFT rules file (default: mullvad.rules)
```

### down
Bring down Mullvad and remove nftables configuration.
``` bash
mnf down [-OPTIONS]:
    -h | --help         Show this help message
    -a | --all          Stop Mullvad and Tailscale/Zerotier (default: only stop Mullvad)
    -z | --zerotier     Use Zerotier instead of Tailscale
    -t | --table        Indicate the nft tablename to bring down (default: mullvad-ts)
```

### conf
Apply nftables configuration so Mullvad and Tailscale/Zerotier can work together and do nothing more.
``` bash
mnf conf [-OPTIONS]:
    -u                  Remove the nftables configuration
    -h                  Show this help message
```

### Example
`mnf up -rz -d 1.1.1.1 -c ee`

or the same command with long flag names:

`mnf up --ram --zerotier --dns 1.1.1.1 --country ee`

This connects to Mullvad's RAM-only servers (`-r`) in Estonia (`-c ee`) and uses Zerotier (`-z`). It also sets the MullvadVPN DNS to `1.1.1.1` .
