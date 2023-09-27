# Edit_Cron

This Ansible playbook disables the logrotate logs for the `logrefresh.sh` script on AWS hosts.

**Requirements:**

* Ansible 2.9 or higher

**Usage:**

```
ansible-playbook -i hosts disable_logrefresh_sh_logs.yml
```

**Example:**

```
ansible-playbook -i hosts -l my_aws_hosts disable_logrefresh_sh_logs.yml
```

**How it works:**

The playbook first checks if the `mstr_logrefresh_sh_logs_disabled` fact is set. If it is not set, the playbook checks if the `logrefresh.sh.log` cron job is present. If it is, the playbook removes the cron job.

The playbook then creates a new cron job that runs the `logrefresh.sh` script with no logs. The cron job is configured to run every 5 minutes.

Finally, the playbook restarts the cron service to ensure that the new cron job is picked up.

**Notes:**

* The playbook is only configured to run on AWS hosts. The `when` clause in the `tasks` section ensures that the tasks are only executed if the `ansible_facts['mstr_cloud_provider']` variable is equal to `AWS`.
* The `cacheable` option in the `set_fact` task ensures that the `mstr_logrefresh_sh_logs_disabled` fact is only calculated once. This can improve performance if the playbook is run multiple times.
