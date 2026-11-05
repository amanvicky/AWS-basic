# AWS — CLI Basics

First pass at AWS through the CLI: EC2 lifecycle, security groups, and the console-vs-CLI workflow.

## Contents

- `AWS+Command+Line+Interface+Part+1.pdf` / `AWSSlides.pdf` — course decks
- `STRESSInstallAndCommands.txt` — install + first commands (configure, ec2 run/describe/terminate)
- `morInEec2_part1.txt` — deeper EC2 notes (AMIs, instances, keypairs)
- `multios_websetup.sh` — multi-OS web server bootstrap script

## Commands I actually use

```bash
aws configure                          # keys + region
aws ec2 run-instances --image-id ami-... --instance-type t2.micro --key-name mykey
aws ec2 describe-instances --query 'Reservations[].Instances[].[InstanceId,State.Name]'
aws ec2 terminate-instances --instance-ids i-...
```

Continued in [Tf-EC2](https://github.com/amanvicky/Tf-EC2) (same infra as code) and [cloudformation-practice](https://github.com/amanvicky/cloudformation-practice).
