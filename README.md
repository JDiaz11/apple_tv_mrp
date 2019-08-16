# Home Assistant Apple TV using MediaRemoteTV Protocol (MRP)

This is an update to the built-in Home Assistant Apple TV component which uses a updated pyatv version which is based on the MediaRemoteTV protocol (MRP). This protocol is necessary in order to support Apple TVs moving forward. A fair amount of changes are necessary in order to support this:

  1) pyatv is not very active anymore. I had to do a bunch of work because they haven't released the latest code on PyPI, so I couldn't import it in normal ways.

  2) Interfacing with the latest pyatv code takes a bunch of updates. That should all be working in this repository.

  3) The MRP repository doesn't seem to work with Home Sharing, so it always needs a pin. I made that happen.

You should be able to install this component as a custom component alongside the normal one using HACS or manually. The only difference in configuration is that the instead of login_id, port is now necessary. And the domain is apple_tv_mrp. So your config will look like this:

apple_tv_mrp:
  - name: Bedroom Apple TV
    host: 10.0.0.6
    port: 49153
    credentials: credentials

Your credentials from the previous version will not work. You will need to re-authenticate using the apple_tv_mrp_authenticate service.