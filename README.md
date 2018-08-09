# The openshift-applier playbooks

## openshift-cluster-seed.yml (openshift-applier)
This playbook is mainly here to serve as the execution point for the [openshift-applier](../roles/openshift-applier). There are few important notes to make about how this is executed:

### 1. Inventory
To better integrate with other tools leveraging this playbook, the `hosts` have been defined as `seed-hosts`. This means that the inventory needs to contain a valid group for `seed-hosts`, such as:

```
[seed-hosts]
localhost
```

### 2. Local Execution
If the playbook is executed locally, i.e.: on your `localhost`, it's recommended to run with the local option ( `--connection=local` ) to speed up the execution. This prevents the inventory from being copied to the remote host (even if the remote host is your localhost) and hence avoids the extra time it takes to iterate over the inventory content to copy the local files.

```
ansible-playbook -i <inventory> path/to/openshift-applier/playbooks/openshift-cluster-seed.yml --connection=local
```

### 3. Custom Secret Definitions
If you'd like to configure custom secrets in the project while executing the s2i-play build, configure a list of dictionaries in `group_vars/all/vars.yml`. Ensure this values are passed to the **b64encode** jinja2 filter. To secure any sensitive information, reference vault vars (`group_vars/all/vault.yml`) from your global `vars.yml` file and execute with an `--ask-vault-pass` or `--vault-password-file=~/.pass`.

```
ansible-playbook -i applier/inventory roles/openshift-applier/playbooks/openshift-cluster-seed.yml --ask-vault-pass
```
