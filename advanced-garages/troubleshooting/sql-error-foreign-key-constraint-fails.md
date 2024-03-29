# SQL error: foreign key constraint fails

<figure><img src="../../.gitbook/assets/image (19).png" alt=""><figcaption><p>Example of the error</p></figcaption></figure>

This is an uncommon issue with some QBCore installations. The way we set up job garages, we update the `citizenid` and set it to the name of the job. Some installations prevent this due to a database constraint.

Fixing this issue is simple, just remove the constraint. Run the following SQL in your database:

```sql
ALTER TABLE player_vehicles DROP FOREIGN KEY FK_playervehicles_players
```
