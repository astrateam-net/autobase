what parameters should I change in order to simply install along with the pgbackrest cluster

```
patroni_cluster_bootstrap_method: "initdb"
patroni_create_replica_methods:
  - basebackup
pgbackrest_install: true
```

---
Just for information (this is not the cause of the problem), it is not necessary to do this, just launch a playbook with a tag to restore the current cluster:

ansible-playbook deploy_pgcluster.yml --tags point_in_time_recovery
This is described here https://github.com/vitabaks/postgresql_cluster?tab=readme-ov-file#restore-and-cloning ("Point-In-Time-Recovery" section)

---
The config_pgcluster.yml playbook is designed specifically for updating configurations and is not intended for deployment tasks, such as creating a stanza.

In your case, you should have executed the deploy_pgcluster.yml playbook with the pgbackrest tag:
```
ansible-playbook deploy_pgcluster.yml -e "postgresql_exists=true" -t pgbackrest
```


```
ansible-playbook deploy_pgcluster.yml -e "enable_pgvector=true"
```
