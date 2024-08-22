# FiveM Escrow Errors

<details>

<summary>Error parsing script... syntax error near &#x3C;\1></summary>

#### Error Example

```log
[script:jg-advancedga] Error parsing script @jg-advancedgarages/server/sv-impound.lua in resource jg-advancedgarages: @jg-advancedgarages/server/sv-impound.lua:1: syntax error near '<\1>'
[    c-scripting-core] Failed to load script server/sv-impound.lua.
```

#### Solutions

* **You are using FileZilla and files have been corrupted during transfer** - try using an alternative FTP client such as [WinSCP](https://winscp.net/eng/index.php)
* You are transferring the folder to your server file by file — **you must upload the .zip file as-is** and then extract it **after** it has been transferred to your VPS
* Your server version is too old, the minimum version is 4752
  * You can download updated server artifacts [here](https://runtime.fivem.net/artifacts/fivem/build\_server\_windows/master/)
  * ...or check out the official [FiveM guide](https://docs.fivem.net/docs/server-manual/setting-up-a-server/)

</details>

<details>

<summary>Failed to verify protected resource</summary>

#### Error Example

```log
[svadhesive] Failed to verify protected resource jg-advancedgarages
```

#### Solutions

* Try restarting your server
* You are transferring the folder to your server file by file — you must upload the .zip file as-is and then extract it **after** it has been transferred to your VPS
* You don't have `.fxap` file in the script folder - try installing the script again
* You are using **File**Zilla and files have been corrupted during transfer - try using an alternative FTP client such as [WinSCP](https://winscp.net/eng/index.php)

</details>

<details>

<summary>You lack the required entitlement</summary>

#### Error Example

```
You lack the required entitlement to use jg-advancedgarages
```

#### What does this mean?

All JG Scripts use the FiveM escrow system, which means scripts are linked to your FiveM account (the account you used on Tebex).

In order to work, the script(s) must be run in a server which is using a server key created by the same FiveM account you used on Tebex. You can create a server key in [FiveM Keymaster](https://keymaster.fivem.net/).

Once you've created a server key, add it to your server's `server.cfg` like this:

```
sv_licenseKey "5594nen725je5bw8s8rkwhahepnmsp9b"
```

#### The script is on my friend's FiveM account

To transfer the script to another account, you can head to:

&#x20;    [FiveM Keymaster](https://keymaster.fivem.net/) -> Purchased assets tab -> Transfer to another account

<mark style="color:red;">cfx.re only allow scripts to be transferred 1 time, so you won't be able to transfer the script again if you do this.</mark>

#### ZAP-Hosting

If you are using a ZAP-Hosting server, you do **not** have to enter your server key in server.cfg, but directly in your server's control panel. Official instructions on how to add your server key can be [found here](https://zap-hosting.com/guides/docs/en/fivem\_licensekey/).

</details>

{% hint style="warning" %}
Make sure you read all the instructions above **very carefully**. If you have tried everything above and are still having issues, make sure you wait for at least 30 minutes if you have just purchased the script, as this can sometimes fix the issue. Also ensure you have done a full server restart.
{% endhint %}
