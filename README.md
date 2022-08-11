# mullvad-tailscale

This is a simple script that allows you to run MullvadVPN along with Tailscale. 

The script will connect you to a random available Mullvad server.

The script is named `mtc` as per `Mullvad Tailscale Connect`. The script was inspired by [this gist](https://gist.github.com/1player/e9cadfef833d5eb5a23c30223f560147) although it now has been completely rewritten.

### Features

- [x] See uage and help
- [x] Bring up Tailscale + Mullvad with a random server
- [x] Blacklist countries to avoid connecting
- [x] Only use [RAM only servers](https://mullvad.net/en/blog/2022/8/1/expanding-diskless-infrastructure-to-more-locations-system-transparency-stboot/) (diskless)
- [x] Set custom DNS server for Mullvad
- [x] Bring down Mullvad VPN and remove nftables entries.
- [x] Bring down all (tailscale+mullvad+nftables)
- [x] Update the relay list at stratup

## Requirements

You should [install mullvad](https://mullvad.net/download/) in your system so you have the [mullvad cli](https://mullvad.net/en/help/how-use-mullvad-cli/) command available.

You will also neeed `nftables` package installed.

Finally, you need `tailscale` with its proper setup.


## Setup

1. Clone this repo:

```bash
git clone https://github.com/r3nor/mullvad-tailscale
```

2. Go to cloned dir: `cd mullvad-tailscale`

3. Set execution permissions on script:

```bash
chmod 700 mtc
```

4. Inspect and **edit** the script file (`mtc`):

- Change the `SCRIPT_DIR` variable to point to the folder where the `mullvad.rules` file is located.

- Modify the `EXCLUDED_COUNTRY_CODES` if you want to exclude any countries from the VPN connection (don't connect to these countries). If you don't want to exclude any CC set this variable to `'(0)'`. If you want to add more, just follow the pattern.

5. Edit the `mullvad.rules` file:

- Set your Tailscale network IPs in the `EXCLUDED_IPS` variable (you can use CDIR notation). 
- Set your Tailscale network IPv6 IPs in the `EXCLUDED_IPV6` variable (you can use CDIR notation). 
- Set your Tailscale DNS resolver in `RESOLVER_ADDRS` (Should be `100.100.100.100`)

6. Setup your Mullvad account if you haven't done it yet:

```bash
mullvad account set 1234123412341234
```

## Usage

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

