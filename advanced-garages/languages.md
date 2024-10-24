# Languages

### Available languages out the box

To use one of the 14 built-in language packs, just select the language in the [configurator](https://configurator.jgscripts.com/advanced-garages).

<table><thead><tr><th width="365">Language</th><th>Code</th></tr></thead><tbody><tr><td>English</td><td>en</td></tr><tr><td>Spanish</td><td>es</td></tr><tr><td>German</td><td>de</td></tr><tr><td>Dutch</td><td>nl</td></tr><tr><td>Danish</td><td>da</td></tr><tr><td>Portuguese</td><td>pt</td></tr><tr><td>Czech</td><td>cs</td></tr><tr><td>Lithuanian</td><td>lt</td></tr><tr><td>Finnish</td><td>fi</td></tr><tr><td>Hungarian</td><td>hu</td></tr><tr><td>Chinese (Simplified)</td><td>cn</td></tr><tr><td>Vietnamese</td><td>vi</td></tr><tr><td>Italian</td><td>it</td></tr><tr><td>Swedish</td><td>sv</td></tr></tbody></table>

### Adding a custom language

1. Create a copy of `locales/en.lua` and rename the file to your chosen language code, for example`de.lua` for German.
2.  On line 3, also change to your new language code, for example for German:

    `Locales['de'] = {`
3. Now, the long part; translate all the strings as necessary. If you see `%{value}`, you must leave this intact or it will break the script! Also, do not change the variable names!

{% hint style="info" %}
Psst :wink: have you translated the script into your native language? Send it to us, and if we add it as an official language for the script we will give you a free [Pro Scoreboard](https://jgscripts.com/qb-esx-pro-scoreboard.html).
{% endhint %}

