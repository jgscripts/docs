# Execution of native... SetDriftTyresEnabled error

<figure><img src="../../.gitbook/assets/image (18).png" alt=""><figcaption><p>Example of the error</p></figcaption></figure>

If you are receiving this error, it's because you are using a game version that is out of date. This code that ESX is trying to use, was introduced in game build 2372 ([https://docs.fivem.net/natives/?\_0x5AC79C98C5C17F05](https://docs.fivem.net/natives/?\_0x5AC79C98C5C17F05))

To fix this, go into your `server.cfg` and add the following line:

```
sv_enforcegamebuild 2944
```
