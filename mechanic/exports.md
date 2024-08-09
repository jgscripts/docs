# Exports

## Server Exports

***

### vehiclePlateUpdated

You need to run this export in all scripts that updates plates, otherwise you will lose your vehicle's data.

<pre class="language-lua"><code class="lang-lua"><strong>-- plate: string
</strong><strong>-- newPlate: string
</strong><strong>
</strong><strong>exports["jg-mechanic"]:vehiclePlateUpdated(plate, newPlate)
</strong></code></pre>
