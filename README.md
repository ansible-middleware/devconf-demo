# Ansible Automation Platform Environment Automation

This repository contains assets to provision and configure VM's on OpenStack and OpenShift environment and deploy Jboss EAP on a cluster. For this demo, we are using Red Hat certified redhead.openshift_virtualization collection to provision the VM instances and configuration on the Openshift environment and openstack.cloud collection to do the provisioning on the Openstack environment.


## Prerequistes

As this demo uses a collections available on [Red Hat Automation Hub](https://www.ansible.com/products/automation-hub), on top of collection coming from Ansible Galaxy, you'll need to properly configure access to both system and provide your credentials for Automation Hub, in the ansible.cfg file:

    [galaxy]
    server_list = automation_hub, galaxy

    [galaxy_server.galaxy]
    url=https://galaxy.ansible.com/

    [galaxy_server.automation_hub]
    url=https://console.redhat.com/api/automation-hub/content/published/
    auth_url=https://sso.redhat.com/auth/realms/redhat-external/protocol/openid-connect/token
    token=<your-token>

Install dependencies via:

    $ pip install -r requirements.txt
    $ ansible-galaxy collection install -r requirements.yml

Create a rhn-creds.yml file containing your RHN account credentials, needed to download packages, as follows:

    rhn_username: '...'
    rhn_password: '...'

## Demo setup

Make sure you have access to Openshift and Openstack environments and they are accessible. For Invetory we are using the dynamic inventory on Openstack and Openshift. Inventory files can be found under inventory/ folder.

Clone the Repository

   $ git clone https://github.com/hcherukuri/devconf-demo.git
   $ cd devconf-demo

Three playbooks compose this demo:

1. create-openshift-resources.yml or create-openstack-resources.yml - This playbook will setup and configure resources on Openstack or Openshift environments respectively.

2. deploy-demo-openshift.yml or deploy-demo-openstack.yml - This playbook will use the dynamic inventory provided and configures all the required softwares and deploys EAP on the cluster of VM and configure it.

3. clean-openstack-resources.yml or  clean-openshift-resources.yml- This playbook will clean off all the resources created for the demo.


Finally run the playbook:


	$ ansible-playbook  -i inventory/openstack.yaml  \
                        -e "ansible_user=<user> ansible_ssh_private_key_file=<path_to_private_key>" -e @rhn-creds.yml \
                 		playbooks/deploy-demo.yml

	$ ansible-playbook  -i inventory/kubevirt.yaml  \
                        -e "ansible_user=<user> ansible_ssh_private_key_file=<path_to_private_key>" -e @rhn-creds.yml \
                 		playbooks/deploy-demo.yml
## License

Apache License v2.0 or later

See [LICENSE](LICENSE) to view the full text.

Author Information
------------------

* [Harsha Cherukuri](https://github.com/hcherukuri)
* [Andrew Block](https://github.com/sabre1041)