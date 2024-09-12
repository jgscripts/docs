# Job Setup

With JG Mechanic, you have the option of using our built-in job system, with a built in society bank account, employees and more. Or, you can use the job system built into your framework, and a third-party society banking script. This page will explain how you can get the right setup for your server.

Only mechanic locations set as `"owned"` can be linked to a job.

### Built-in job system

Set `Config.UseFrameworkJobs = false`

Accessing the built-in society banking, employee management and other features is done through the tablet. With `Config.AdminsHaveEmployeePermissions = true` you can log into the tablet as an admin (/tablet) and manage the mechanic locations yourself; but the preferred way would be to set an _initial owner_ of the mechanic, so they will have access to the tablet and can hire further employees themselves. This could be through the form of them 'buying' the business.

To set the initial owner, use /mechanicadmin and click "Set owner" on the corresponding location (remember it must be set to `"owned"` in config.lua and NOT `"self-service"`.

The now owner of the mechanic location can use /tablet to log in and manage the business by themselves. All functions can be accessed through the 'Management' app.

### Framework job system (built into Qbox/QBCore/ESX)

Set `Config.UseFrameworkJobs = true`

The location MUST be set up with a type of `"owned"` (NOT `"self-service"`), and it needs to have a unique `job` set. The job is the corresponding job name in your framework. If you want to have unique businesses, you will need to create unique job names within your framework. Here is an example of what a correctly configured location should include:

```lua
type = "owned",
job = "mechanic",
jobManagementRanks = {3, 4}, -- which ranks should have access to 'ownership' perms
```

Once setup correctly, changing your job to the job name configured will allow you to login in the tablet (/tablet).

### Society banking (only with `Config.UseFrameworkJobs = true`)

Out of the box, we support `"okokBanking"`, `"fd_banking"`, `"Renewed-Banking"`, `"qb-banking"`, `"qb-management"`, `"esx_addonaccount"`.

Simply set `Config.SocietyBanking` to one of the systems above.

{% hint style="warning" %}
If you're using `esx_addonaccount` and experiencing errors, please try the following guide: [esx-framework-jobs.md](installation-and-configuration/esx-framework-jobs.md "mention")
{% endhint %}

If you would like to use a custom society banking system, you will need to add the exports into the following server functions, found in `framework/sv-functions.lua`

```lua
function Framework.Server.GetSocietyBalance(society, societyType)

function Framework.Server.PayIntoSocietyFund(societyName, societyType, amount)

function Framework.Server.RemoveFromSocietyFund(societyName, societyType, amount)
```

&#x20;&#x20;
