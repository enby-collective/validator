# Setup instructions

Setup instructions for Validators provided by enby collective (incomplete!)

## Prepare your validator

-  ssh into your validator: `ssh root@IP_ADDRESS_VALIDATOR`
- `adduse ansible_user`
- `usermod -aG sudo ansible_user`
- `visudo` (alternatively `sudo ap-get install nano && sudo Editor=nano visudo`) \
   add the following last line: \
   ansible_user ALL=(ALL) NOPASSWD:ALL
- logout of your validator / shh connection
- `ssh-copy-id -i /home/achim/.ssh/id_rsa.pub ansible_user@IP_ADDRESS_VALIDATOR`

## Prepare your Ansible setup:

- `sudo apt-get install ansible`
- `ansible-galaxy collection install community.general`
- `gh clone https://github.com/enby-collective/polkadot-validator`
- `cd polkadot-validator`
- `cp inventory.sample inventory`
- edit `inventory` file like this: 

  ```
    [kusama1]
    IP_ADDRESS_VALIDATOR validator_name=NAME_VALIDATOR log_name=kusama1 telemetryUrl=wss://telemetry-backend.w3f.community/submit/

    [kusama:children]
    kusama1

    [validators:children]
    kusama

    [all:vars]
    ansible_user=ansible
    ansible_port=22
    ansible_ssh_private_key_file="~/.ssh/id_rsa"
    log_monitor=IP_ADDRESS_LOGGING_SERVER
  ```
  
- update the value of `polkadot_db_snapshot_url` under `group_vars/kusama.yml` with the lates snapshot from https://polkachu.com/snapshots/kusama
- update the value of `polkadot_version` under `group_vars/validators.yml` with the latest polkadot version


## Start the Ansible setup
- enter: `ansible-playbook -i inventory polkadot_full_setup.yml -e "target=kusama1"`
