# mullvad tailscale / zerotier
<p align="center"><b>Mullvad NF Tables Script</b></p>

> Note: The script was formerly called `mtc`. It is now `mnf` and has been improved.

This is a simple bash script that will allow you to run MullvadVPN along with Tailscale or Zerotier in a Linux system.

The script is named `mnf` as per `Mullvad NF Tables`. The script was inspired by [this gist](https://gist.github.com/1player/e9cadfef833d5eb5a23c30223f560147), although it now has been completely rewritten and improved.

The script can work with Zerotier and Tailscale. It would most probably also work with any other VPN like NetMaker of Wireguard, but I have not tested it.

## Features

- [x] Complete usage guide (and help)
- [x] Bring up Tailscale/Zerotier + Mullvad with a random server
- [x] Connect to a specific country via country code.
- [x] Select a specific rules file
- [x] Select a specific nf table
- [x] Blacklist countries to avoid connecting to them
- [x] Use only [RAM-only (no disk) servers](https://mullvad.net/en/blog/2022/8/1/expanding-diskless-infrastructure-to-more-locations-system-transparency-stboot/)
- [x] Set custom DNS server for Mullvad
- [x] Bring down Mullvad VPN and remove nftables entries.
- [x] Bring down all (tailscale/zerotier+mullvad+nftables)
- [x] Automatically update the relay list at stratup
- [x] Only apply nftables configuration and do nothing more
- [x] Only remove nftables configuration and do nothing more

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
chmod 700 mnf
```

4. Inspect and **edit** the script file (`mnf`):

- Change the `RULES_DIR` variable to point to the folder where the `mullvad.rules` file from this repository is located. If you cloned the repo, it should be inside the `mullvad-tailscale` folder. Please, make sure you add the trailing slash (slash at the end).

- Modify the `EXCLUDED_COUNTRY_CODES` if you want to exclude any countries from the VPN connection (don't connect to these countries). If you do not want to exclude any CC set this variable to `'(none)'`. If you want to add more, just follow the pattern.

5. Edit the `mullvad.rules` file:

- Set your Tailscale/Zerotier network IPs in the `EXCLUDED_IPS` variable (you can use CDIR notation). 
- Set your Tailscale/Zerotier network IPv6 IPs in the `EXCLUDED_IPV6` variable (you can use CDIR notation), leave it blank (`= ""`) if there are no IPv6s.
- Set your Tailscale/Zerotier DNS resolver in `RESOLVER_ADDRS`.
    - Should be `100.100.100.100` for Tailscale.
    - If you are using Zerotier, the DNS resolver can be found in the `/etc/resolv.conf` file after running `zerotier-one` service. You will find it in a new line, it should look something like `10.X.X.X`.

> You can find the Tailscale/Zerotier IPs in your dashboard. Just copy and paste for each of your devices.


6. Setup your Mullvad account if you haven't done it yet:

```bash
mullvad account login 1234123412341234
```

## Usage

For Zerotier users, you should apply `-z` flag on all `up/down` actions.

> You must be inside the directory where the script is located, or use it with the absolute path to it. If you want to run the command without specifying the folder where it is located, add the script directory to your PATH variable.

### up
Apply nftables configuration and connect to Mullvad and Tailscale/Zerotier.

- mnf up [-OPTIONS]:
    - -h, --help:         Show this help message.
    - -r, --ram:          No-disk/RAM only Mullvad relays (default: all servers)
    - -z, --zerotier:     Use Zerotier instead of Tailscale
    - -d, --dns:          Set custom Mullvad DNS Server (i.e. -d 1.1.1.1)
    - -c, --country:      Specify a country code to connect to (i.e. -c gb)
    - -f, --file:         Specify a particular NFT rules file (default: mullvad.rules)

### down
Bring down Mullvad and remove nftables configuration.

- mnf down [-OPTIONS]:
    - -h, --help:         Show this help message.
    - -a, --all:          Stop Mullvad and Tailscale/Zerotier (default: only stop Mullvad)
    - -z, --zerotier:     Use Zerotier instead of Tailscale
    - -t, --table:        Indicate the nft tablename to bring down (default: mullvad-ts)

### conf
Apply nftables configuration so Mullvad and Tailscale/Zerotier can work together and do nothing more.

- mnf conf [-OPTIONS]:
    - -u: Remove the nftables configuration.
    - -h: Show this help message.
