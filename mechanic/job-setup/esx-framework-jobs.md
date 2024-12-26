---
description: How to correctly add the society jobs for esx.
---

# ESX Framework Jobs

{% hint style="info" %}
You only need to follow these steps if you're usin&#x67;**`Config.UseFrameworkJobs = true`**
{% endhint %}

Follow these steps for all owned mechanics.

1.  Get the job name from the config.lua\


    <figure><img src="../../.gitbook/assets/image (26).png" alt=""><figcaption></figcaption></figure>
2. Go to your database.\

3.  Open the `addon_account` table\


    <figure><img src="../../.gitbook/assets/image (28).png" alt=""><figcaption></figcaption></figure>
4.  Right click and press Insert row\


    <figure><img src="../../.gitbook/assets/image (29).png" alt=""><figcaption></figcaption></figure>
5.  Set the name to `society_[JOBNAME]`

    For example: job name `lscustoms` would be `society_lscustoms`\
    Set the `shared` column to 1\

6.  Open the addon\_account\_data table\
    \


    <figure><img src="../../.gitbook/assets/image (30).png" alt=""><figcaption></figcaption></figure>
7.  Right click and press Insert row\


    <figure><img src="../../.gitbook/assets/image (31).png" alt=""><figcaption></figcaption></figure>
8. Set the account\_name to the same as before eg: `society_lscustoms`\
   Then set `money` to 0 and leave the rest as it is.

And now you are all set! If you have any questions please use the mechanic chat or ticket.
