# Workflow test repo

This is my personal collection of Ansible Workflow examples. Of course, most these are recycled from other repositories.

## Running this

The [configure_tower.yml](configure_tower.yml) will configure your Ansible Tower instance to setup this repo as a Project and create the Job Templates and Workflow required for the demo documented [here](TODO).

`tower_hostname` and `tower_oauthtoken` need to be provided as extra vars or Tower Credentials. Check out [Creating an OAuth 2 token](https://docs.ansible.com/ansible-tower/latest/html/userguide/applications_auth.html#ug-tokens-auth-create).

The Ansible Tower collection (or AWX) namespace needs to be referenced.

```yaml
   hosts: localhost
   collections:
     - ansible.tower
```

The configuration details are loaded from the `yml` files in the [vars](vars) folder.

```yaml
   pre_tasks:
     - name: Include vars from vars directory
       include_vars:
         dir: ./vars
         extensions: ["yml"]
```

Finally the [redhat_cop.tower_configuration](https://github.com/redhat-cop/tower_configuration) collection send these to Ansible Tower. This includes `projects`, `job_templates` and `workflows`.

```yaml
   roles:
       - {role: redhat_cop.tower_configuration.projects, when: tower_projects is defined}
       - {role: redhat_cop.tower_configuration.job_templates, when: tower_templates is defined}
```

## Dependencies

All dependencies are included in the [requirements.yml](collections/requirements.yml) file. You can either use the AWX Collection `awx.awx`, which is the upstream community distribution available on [Ansible Galaxy](https://galaxy.ansible.com/awx/awx) or the downstream supported Ansible Collection `ansible.tower` that is available on [Automation Hub](https://cloud.redhat.com/ansible/automation-hub/repo/published/ansible/tower).

```bash
ansible-galaxy collection install -r collections/requirements.yml
```

## API (temp)

https://www.ansible.com/blog/tower-workflow-convergence
`https://tower.nleiva.com/api/v2/workflow_job_nodes/?job_id=2581`
`https://tower.nleiva.com/api/v2/workflow_job_nodes/?success_nodes=313`

variables: https://docs.ansible.com/ansible-tower/latest/html/userguide/job_templates.html#launch-a-job-template