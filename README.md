# Home Assistant Apple TV using MediaRemoteTV Protocol (MRP)

This is an update to the built-in Home Assistant Apple TV component which uses a updated pyatv version which is based on the MediaRemoteTV protocol (MRP). This protocol is necessary in order to support Apple TVs moving forward. A fair amount of changes are necessary in order to support this:

1. pyatv is not very active anymore. I had to do a bunch of work because they haven't released the latest code on PyPI, so I couldn't import it in normal ways.

2. Interfacing with the latest pyatv code takes a bunch of updates. That should all be working in this repository.

3. The MRP repository doesn't seem to work with Home Sharing, so it always needs a pin. I made that happen.

You should be able to install this component as a custom component alongside the normal one using HACS or manually. The only difference in configuration is that the instead of login_id, port is now necessary. And the domain is apple_tv_mrp.

**Your credentials from the previous version will not work. You will need to re-authenticate using the apple_tv_mrp_authenticate service.**

## Full steps

1. Install using HACS or manually to custom_components folder

2. Add the `apple_tv_mrp` component to your configuration with no further configuration:

```yaml
apple_tv_mrp:
```

3. Run the `apple_tv_mrp.apple_tv_mrp_scan` service and locate the notification. Copy the results elsewhere.

![Scan Results](https://i.imgur.com/Ek5HMhs.png)

4. One at a time, add an entry to your configuration like so, using the values from your scan:

```yaml
apple_tv_mrp:
  - name: Bedroom Apple TV
    host: 10.0.0.6
    port: 49154
```

5. After adding each value, restart Home Assistant and a new entity will be added, but it won't work!

6. Run the `apple_tv_mrp.apple_tv_mrp_authenticate` service with the `entity_id` of the new `media_player` entity. Your Apple TV will show a PIN on it:

![Apple TV PIN](https://i.imgur.com/Sbv8OvH.jpg)

7. Open the notification which is asking you to configure you Apple TV and enter the PIN

![Enter PIN](https://i.imgur.com/UDvWrpB.png)

8. You will get another notification that has the credentials. **Note: the text will be longer than the notification can display! It is four segments long, separated by colons. You must select all the text. Triple-clicking is normally sufficient**

![Credential Notification](https://i.imgur.com/0fchhlj.png)

9. Add the credential to your config and restart Home Assistant.

```yaml
apple_tv_mrp:
  - name: Bedroom Apple Tv
    host: 10.0.0.6
    port: 49154
    credentials: 99c702572cb521018274f7136aeeefb8e57dd06cc71cff634752664ecc65f920:6db44955cae5b8ea00cc8094243fbde99e1ebdca7ccebffac7fa08b5b69c7de6:31383466663663312d653362612d343635662d613734632d386462633965306534346665:35343030303232362d326531382d343964382d613039392d383331316232613131386536
```

10. You should now have a working `media_player` and `remote` entity.

11. Repeat steps 4 - 10 for each Apple TV. **Do not re-scan! Use your original scan results!**