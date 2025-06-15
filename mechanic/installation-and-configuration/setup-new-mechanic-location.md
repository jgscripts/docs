---
description: How to correctly create a new mechanic
---

# Setup New Mechanic Location

<details>

<summary><strong>Owned Mechanic</strong></summary>

1. Navigate to `/jg-mechanic/config/config.lua`and locate line 135:

![](<../../.gitbook/assets/image (2) (1).png>)

2. This is a decently large section. We are going to highlight and copy from line `135`to line `229`

3) Line 228 should be the second last bracket with in the `Config.MechanicLocations` table. Add a `,` to the end of the bracket `},`:

![](<../../.gitbook/assets/image (4) (1).png>)

4. Now we're going to paste the copied lscustoms under line 228 to add a duplicate entry

5) Change the name of the newly added lscustoms to your desired name and change the job, locations, shops and stashes coords
6) Save and Restart jg-mechanic. You have now successfully added a new owned location

</details>

<details>

<summary><strong>Self-Service Mechanic</strong></summary>

1. Navigate to `/jg-mechanic/config/config.lua`and locate line 106:

![](<../../.gitbook/assets/image (5).png>)

2. We are now going to highlight and copy from line `106`to line `134`:

3) Now navigate to line 228, this should be the second last bracket of the `Config.MechanicLocations` table, Add a `,` to the end of the bracket `},`:

![](<../../.gitbook/assets/image (4) (1).png>)

4. Now we're going to paste the copied bennys under line 228 to add a duplicate entry

5) Change the name of the newly added bennys to your desired name and change the job and locations
6) Save and Restart jg-mechanic. You have now successfully added a new owned location

</details>

