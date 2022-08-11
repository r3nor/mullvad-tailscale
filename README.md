# mullvad-tailscale

This is a simple script that allows you to run MullvadVPN along with Tailscale. 

The script will connect you to a random available Mullvad server. I will also implement the option to connect to a certain country in a near future.

The script is named `mtc` as per `Mullvad Tailscale Connect`. The script was inspired by [this gist](https://gist.github.com/1player/e9cadfef833d5eb5a23c30223f560147).

## Requirements

You should [install mullvad](https://mullvad.net/download/) in your system so you have the [mullvad cli](https://mullvad.net/en/help/how-use-mullvad-cli/) command available.

You will also neeed `nftables` package installed.

Finally, you need `tailscale` with its proper setup.


## Installation

1. Clone this repo:

```bash
git clone https://github.com/r3nor/mullvad-tailscale
```

2. Go to cloned dir: `cd mullvad-tailscale`

3. Set execution permissions on script:

```bash
chmod 700 mtc
```

4. Inspect and **edit** the script file:

You should change the `SCRIPT_DIR` variable to point to the folder where the `mullvad.rules` file is located.

You can also modify the `EXCLUDED_COUNTRY_CODES` if you want to exclude any countries from the VPN connection (don't connect to these countries). If you don't want to exclude any CC set this variable to `'(0)'`. If you want to add more, just follow the pattern.

```bash
nano mtc
```

5. Set Mullvad account:

```bash
mullvad account set 1234123412341234
```

6. Edit the `mullvad.rules` file:

> Set your Tailscale IPs in the `EXCLUDED_IPS` variable (you can use CDIR notation). Set the IPv6 IPs in `EXCLUDED_IPV6`. Finally set your Tailscale DNS resolver in `RESOLVER_ADDRS`

## Usage

  - To see help: `bash mtc`
  - To start mullvad+tailscale: `bash mtc up`
    - To start mullvad+tailscale with RAM servers only: `bash mtc up -r`
    - To set a custom DNS for Mullvad: `bash mtc up -d 1.1.1.1`
    - You can apply all options at once: `bash mtc up -r -d 8.8.8.8`
  - To stop mullvad (not tailscale): `bash mtc down`
    - To stop all (tailscale included): `bash mtc down -a`

