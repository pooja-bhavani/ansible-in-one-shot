# Ansible In One Shot — TrainWithShubham

Learn Ansible hands-on with real AWS infrastructure provisioned by Terraform.

## Prerequisites

- [Terraform](https://developer.hashicorp.com/terraform/tutorials/aws-get-started/install-cli) (>= 1.6)
- [Ansible](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html) (>= 2.16)
- [AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html) (v2)

---

## Quick Start

```bash
# 1. Clone and enter the repo
git clone https://github.com/TrainWithShubham/ansible-in-one-shot.git
cd ansible-in-one-shot

# 2. Generate SSH key pair
mkdir -p ~/keys
ssh-keygen -t rsa -b 4096 -f ~/keys/terra-key-ansible.pem -N ""
chmod 400 ~/keys/terra-key-ansible.pem
cp ~/keys/terra-key-ansible.pem.pub terraform/terra-key-ansible.pub

# 3. Provision infrastructure
cd terraform && terraform init && terraform apply -auto-approve && cd ..

# 4. Test connectivity
ansible all -m ping
```

---

## Course Modules

| # | Module | What You'll Learn | Est. Time |
|---|--------|-------------------|-----------|
| 1 | [Basics](modules/01-basics/) | Ad-hoc commands, playbooks, packages, services | 2h |
| 2 | [Variables & Facts](modules/02-variables-and-facts/) | vars, register, debug, group_vars, when conditionals | 2h |
| 3 | [Templates & Handlers](modules/03-templates-and-handlers/) | Jinja2 templates, handlers, notify | 1.5h |
| 4 | [Loops & Conditionals](modules/04-loops-and-conditionals/) | loop, when, block/rescue/always, tags | 1.5h |
| 5 | [Roles](modules/05-roles/) | Role structure, site.yml, reusable automation | 2h |
| 6 | [Vault](modules/06-vault/) | ansible-vault, encrypting secrets, no_log | 1h |

**Total: ~10 hours** — Each module has its own README with commands to run.

---

## Infrastructure

Terraform creates **4 EC2 instances** in `us-west-2` (all `t2.micro`, free tier eligible):

| Host | OS | SSH User |
|------|----|----------|
| `master-ubuntu` | Ubuntu 24.04 | `ubuntu` |
| `worker-ubuntu` | Ubuntu 24.04 | `ubuntu` |
| `worker-redhat` | RHEL 9 | `ec2-user` |
| `worker-amazon` | Amazon Linux 2023 | `ec2-user` |

AMI IDs are region-specific — update them in `terraform/variables.tf` if you change regions.

Terraform auto-generates the Ansible inventory at `inventories/dev/hosts.ini`.

---

## Repo Structure

```
ansible-in-one-shot/
├── ansible.cfg               # Ansible settings (inventory, SSH, output)
├── requirements.yml          # Galaxy dependencies
├── terraform/                # EC2, security groups, auto-inventory
├── inventories/dev/          # Inventory + group_vars + host_vars
├── roles/                    # common, docker, nginx
├── modules/                  # 6 learning modules (start here!)
└── solutions/                # Exercises + hints
```

---

## Common Commands

```bash
# Test connectivity
ansible all -m ping

# Run a playbook
ansible-playbook modules/01-basics/01_ping.yml

# Run with verbose output
ansible-playbook modules/01-basics/02_gather_facts.yml -v

# Run specific tags only
ansible-playbook modules/04-loops-and-conditionals/05_tags_demo.yml --tags install

# Dry run (check mode)
ansible-playbook modules/01-basics/03_install_packages.yml --check
```

---

## Troubleshooting

| Problem | Fix |
|---------|-----|
| `Permission denied (publickey)` | Check SSH key path and `chmod 400` |
| `No hosts matched` | Run `terraform apply` to generate inventory |
| `Timeout connecting` | Check security group allows port 22 |
| `become: permission denied` | Default EC2 users have sudo — check your `hosts` line |

---

## Cleanup

```bash
cd terraform && terraform destroy -auto-approve
```

---

## Maintainer

**Shubham Londhe** — [TrainWithShubham](https://github.com/LondheShubham153)
