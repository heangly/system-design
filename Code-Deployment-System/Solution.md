# Solution

<hr>

- **Queue** : for queing all the jobs with FIFO fashion.
  <br/>
- **Servers/Workers**: take the job off from the **Queue** build the code into binary and store in **Blob Store** e.g. GCS (google cloud storage)
  <br/>
- However, if you store your jobs in **Queue**, the jobs will lose status when the power is down, which makes in-memory **Queue** datastructure not a good idea.
  <br/>
- **SQL**: We can use **SQL** as the queue. A table can represent a queue where every job is stored in that table. You would take job off the table.

| id  | name | SHA | created_at | status | last_heart_beat |
| :-- | :--: | --: | ---------: | -----: | --------------- |
| sth | sth  | sth |        sth |    sth | sth             |

```SQL
  BEGIN TRANSACTIONS;
  SELECT * FROM jobs WHERE status = "QUEUED"
  ORDER BY created_at ASC LIMIT 1;

  #Once you have the job, you have to update it
  #If there is no job, you can roll back
  UPDATE jobs SET status = "RUNNING" WHERE id = our_id;

  COMMIT;
```

- **What happen when the job is running and the worker dies in the middle of the process?**
  - We can have **Health Check**
  - When running the jobs, Workers/Servers keep sending **Heart Beat** to **Table**
  - We updated the **last_heart_beat**
  - Auxilaries workers can check on **table** and the **last_heart_beat** to see if there is any workers that do not send the last heart beat over sometimes. Then, we can update the status to **Failed**

<br>

- **So how many workers we need? if we have 5000 build/day and a worker can do 100 build/day**
  - 5000 / 100 = 50 -> so we need 50 workers

<br>

- This design can be scaled both horizontally and vertically

<br>

- Once we succefully build. We can upload it to GCS. Once it is uploaded, we can update the status in SQL to **Success**

<br>

- **We don't want a single blob store in a single region. What should we do?**
  - We can have regional clusters/ sub systems to handle the final part of the deployment.
  - If we have 5 regions, we can replicate that build 5 times from the original blob store.

<br>

- **We only deploy the build only if the sub region already completed replication. What should we do?**
  - We can have a seperated service to check the status of the replication. See if all sub clusters have replicated the new build. Then, we can deploy the build.
  - We have another database to keep track of **status** if it is deployable.

<br>

- **How did it get deployed?**
  - We can do **peer to peer network**.
  - All machines in 1 region cluster, and through **peer to peer network** they can download the build very fast.
  - When engineer presses **build** -> **server** will trigger the whole deployment. We want our workers to know that they can start download the binary
