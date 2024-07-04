# Importing Vehicles

Type `/dealeradmin` into chat, click vehicles, and then the yellow import button on the top right.

#### QB

Importing from the QBCore option will pull from `qb-core/shared/vehicles.lua` . This will only work if the file is valid, and vehicles will be added to specific dealerships if a dealership id in the `config.lua` file matches the `shop` inside of shared.

#### ESX

Importing from the ESX option will pull from the `vehicles` database table. Vehicles will be automagically added to dealerships based on the categories assigned to dealerships from within `config.lua`

`QBOX`

Importing from the QBOX option will pull from `qbx_core/shared/vehicles.lua` . This will only work if the file is valid, and vehicles will be added to specific dealerships if a dealership id in the `config.lua` file matches the `shop` inside of shared.\
(please note that you may need to add shop = `pdm` etc to the vehicles.lua for them to be assigned to the appropriate dealership
