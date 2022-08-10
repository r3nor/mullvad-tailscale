# mullvad-tailscale

This is a simple script that allows you to run MullvadVPN along with Tailscale. 

The script will connect you to a random available Mullvad server. I will also implement the option to connect to a certain country in a near future.

The script is named `mtc` as per `Mullvad Tailscale Connect`. The script was inspired by [this gist](https://gist.github.com/1player/e9cadfef833d5eb5a23c30223f560147).

## Usage

You should [install mullvad](https://mullvad.net/download/) in your system so you have the [mullvad cli](https://mullvad.net/en/help/how-use-mullvad-cli/) command available.

You will also neeed `nftables

1. Clone this repo:

```bash
git clone https://github.com/r3nor/mullvad-tailscale
```

2. Go to cloned dir: `cd mullvad-tailscale`

3. Set execution permissions on script:

```bash
chmod 700 mtc
```

4. Inspect and edit the script:

> Change the top variable to fit your system. Inspect the code, tweak if you want.

```bash
nano mtc
```

5. Set Mullvad account:

```bash
mullvad account set 1234123412341234
```

6. Edit the `mullvad.rules` file:

> Set your Tailscale IPs in the `EXCLUDED_IPS` variable (you can use CDIR notation). Set the IPv6 IPs in `EXCLUDED_IPV6`. Finally set your Tailscale DNS resolver in `RESOLVER_ADDRS

7. Run the script:

```bash
./mtc up
```

or

```bash
bash mtc up
```
