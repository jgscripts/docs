# SQL error: foreign key constraint fails

"Cannot add or update a child row: a foreign key constraint fails...."

<figure><img src="../../.gitbook/assets/image (19).png" alt=""><figcaption><p>Example of the error</p></figcaption></figure>

This is a common issue with some QBCore installations. The way we set up job garages, we update the `citizenid` and set it to the name of the job. Some installations prevent this due to a database constraint.

Fixing this issue is simple, just remove the constraint. Run the following SQL in your database:

```sql
ALTER TABLE player_vehicles DROP FOREIGN KEY FK_playervehicles_players
```

Some databases might use another name than the "FK\_playervehicles\_players" so make sure you change it to the one showing in your error

<figure><img src="../../.gitbook/assets/image (34).png" alt=""><figcaption><p>Example of another foreign key name</p></figcaption></figure>

In this example the foreign key name is `player_vehicles_ibfk_1` so the sql query will look like this:

```sql
ALTER TABLE player_vehicles DROP FOREIGN KEY player_vehicles_ibfk_1
```
