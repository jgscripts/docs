# Translations

## Setting the language in our scripts

Every script from JG Scripts is fully translatable out of the box (except Scoreboard). Simply head to `config.lua`, and update the `Config.Locale` option. You can find a list of available translations inside the `locales` folder.

The name of the file (without the .lua extension) is the value you set `Config.Locale` to. For example, if you see a locale file called `de.lua`, update the config to `Config.Locale = "de"`

## Contributing

All translations for all our scripts are handled from the translations repository, found on GitHub:

{% embed url="https://github.com/jgscripts/translations" %}

Before each new release, all the latest translations are automatically pulled from this repository to ensure our scripts are up to date with the best translations available.

We would love for you to contribute! If you want to contribute a new locale file, or improve an existing one, simply make a new **Pull Request** with your changes, and one of the team will review it.

**Guidelines:**

* The name of the locale file must be named with the language code, and be a lua file. For example `en.lua`, `fr.lua`, `de.lua` and so on.
* When creating a new locale file, simply copy an existing one (recommended to use English as it's the original file created by us), and replace the translations with the ones there.
*   The start of the file must keep the variable declaration, and you must replace the existing language code to match the name of the file.

    <figure><img src=".gitbook/assets/image (1) (1).png" alt=""><figcaption></figcaption></figure>
* Test the file before submitting your Pull Request to ensure that there are no syntax errors.
