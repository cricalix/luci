# LuCI App AdGuardHome

This LuCI app provides basic integration with the [AdGuard Home](https://github.com/AdguardTeam/AdGuardHome) [package](https://openwrt.org/packages/pkgdata/adguardhome) for OpenWrt. Note that the AdGuard Home package installation and configuration requires interaction with the OpenWrt command line; this app does not remove any of that interaction.

See also: [AdGuard Home @ AdGuard](https://adguard.com/en/adguard-home/overview.html)

## Using/installing this app

First, install the AdGuard Home package - either via the web UI for software package management, or
```
opkg install adguardhome
```

Follow the [installation instructions](https://openwrt.org/docs/guide-user/services/dns/adguard-home) for AdGuard Home, and make a note of the username and password for authenticating to the web UI.

(In short, AdGuard Home's setup wizard should be on port 3000, and you must complete the wizard before installing this LuCI app.)

Next, install this package - either via the web UI for software package management, or
```
opkg install luci-app-adguardhome
```

This package is unable to automatically determine the username and password (the password is encrypted in AdGuard Home's configuration file), so you'll need to go to `Services > AdGuard Home > Configuration` and provide these credentials. The credentials will be stored **unencrypted** in `/etc/config/adguardhome`.

With the credentials saved, the `Services > AdGuard Home > Status` page should now work, and show you the general status of AdGuard Home.

If you go to `Services > AdGuard Home > Logs`, you can see the last 50 log lines from both the supporting script used by this package, and the AdGuard Home software.

This app provides a link to the AdGuard Home web UI, making it easy to see more detailed statistics, and the query log.

## How it works

The Lua code in `rpcd/luci.adguardhome` makes REST API calls to the AdGuard Home service defined by `/etc/adguardhome.yaml` on your OpenWrt-enabled router, and renders the responses.

## Dependencies

Dependencies are declared in the Makefile, but are

* adguardhome, as this app is useless without it
* luasocket, for talking to the AdGuard Home REST API
* luci-lib-jsonc, for doing JSON encoding
* lyaml, for parsing the AdGuard home YAML configuration

# Developing

* JS code is formatted with `js-beautify -t -a -j -w 110 -r <filename>`
* Lua is formatted with `stylua --indent-type Spaces --sort-requires <filename>`

# Troubleshooting

AdGuard Home's configuration file has changed over time; future changes may break the keys that this app looks for when trying to determine the host and port for the REST API calls. If this happens, an error message referring to http.address will be rendered. An update to this app will be required to solve that - please file a bug on GitHub, and if possible include /etc/adguardhome.yaml.
