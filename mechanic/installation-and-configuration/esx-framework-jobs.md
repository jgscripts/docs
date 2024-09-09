---
description: How to correctly add the society jobs for esx.
---

# ESX Framework Jobs

{% hint style="info" %}
You only need to follow these steps if you're using**`Config.UseFrameworkJobs`**
{% endhint %}

\
\
Follow these steps for all owned mechanics.



1. Get the job name from the config.lua\
   ![](<../../.gitbook/assets/image (26).png>)\

2. Go to your database.\

3. Open the `addon_account` table\
   ![](<../../.gitbook/assets/image (28).png>)\

4. Right click and press Insert row\
   ![](<../../.gitbook/assets/image (29).png>)\

5. Set the name to `society_"JOBNAME"`\
   eg the job name `lscustoms` would be `society_lscustoms`\
   Set a proper label name and then set shared to 1\

6. Open the addon\_account\_data table\
   ![](<../../.gitbook/assets/image (30).png>)\

7. Right click and press Insert row\
   ![](<../../.gitbook/assets/image (31).png>)\

8. Set the account\_name to the same as before eg: `society_lscustoms`\
   Then set `money` to 0 and leave the rest as it is.

And now you are all set.\
If you have any questions please use the mechanic chat or ticket.
