# Setup instructions

Setup instructions for Validators provided by enby collective (incomplete!)

## Prepare your validator

-  ssh into your validator: `ssh root@IP_ADDRESS_VALIDATOR`
- `adduser ansible`
- `usermod -aG sudo ansible`
- `visudo` (alternatively `sudo ap-get install nano && sudo Editor=nano visudo`) \
   add the following last line: \
   ```bash
   ansible ALL=(ALL) NOPASSWD:ALL
   ```
- logout of your validator / shh connection
- `ssh-copy-id -i ~/.ssh/id_rsa.pub ansible@IP_ADDRESS_VALIDATOR`

## Prepare your Ansible setup:

- `sudo apt-get install ansible`
- `ansible-galaxy collection install community.general`
- `gh clone https://github.com/enby-collective/validator.git`
- `cd validator`
- `cp inventory.sample inventory`
- edit your `inventory` file and update it as follows : 
   - replace the `IP_ADDRESS_VALIDATOR` with the IP of your server
   - in the `domain_name` add the Hostname of your server
   - in the `letsencrypt_email` add your email
   - give your assigned values in `validator_name`, `log_name`, under `[kusama:children]`, `loki_password` and `data_password`
- Open the `group_vars/kusama.yml` file and update the value of `polkadot_db_snapshot_url` so that it reflects the latest snapshot from https://polkachu.com/snapshots/kusama
- Open the `group_vars/validators.yml` file and update the value of the `polkadot_version` with the version that correspond to the [latest polkadot release](https://github.com/paritytech/polkadot/releases).


## Start the Ansible setup
- Before running the command `ansible-playbook -i inventory polkadot_full_setup.yml -e "target=kusama1"` make sure to update the value of `target` to the kusama name that corresponds to your server, e.g. kusama2 or kusama3, etc.
