Sorry for the delay, there was no way to try to reproduce the problem.

Today I tried the following scenario:

Deployed the cluster from scratch
Set up backups
Set the following vars/main settings and run ansible-playbook config_pgcluster.yml
patroni_cluster_bootstrap_method: "pgbackrest"  # or "wal-g", "pgbackrest", "pg_probackup"

```
patroni_create_replica_methods:
  - pgbackrest
```
I made a Full backup, added test tables within an hour, filled them with data
I waited about 30 minutes until the WAL files were sent to the backup host
I started the recovery scenarios:

Without deleting the cluster, I deleted the previously created databases, but I did not delete the cluster.
1.1) Deleted the test bases
1.2) I launched the ansible-playbook deploy_pgcluster.yml - The recovery went without problems, the databases were restored at the time I needed
in vars/main pgbackrest_patroni_cluster_restore_command
```
pgbackrest_patroni_cluster_restore_command:
#  '/usr/bin/pgbackrest --stanza={{ pgbackrest_stanza }} --delta restore'  # restore from latest backup
  '/usr/bin/pgbackrest --stanza={{ pgbackrest_stanza }} --type=time "--target=2024-04-16 12:12:00+03" --delta restore'  # Point-in-Time Recovery (example)
```  
Deleted the cluster via the remove_cluster.yml playbook
2.1) Launched remove_cluster.yml
2.2) Fixed postgresql_exists=false in the inventory
2.3) Launched deploy_pgcluster.yml
2.4) Got the working cluster restored for the right time
