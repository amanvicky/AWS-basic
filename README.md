# AWS — CLI Basics, And the Stress That Came With It

First pass at AWS through the CLI, centered on EC2: launch, inspect, break, terminate — with the console open in one tab and `aws` in the other to compare what each does.

## Contents

| File | What it holds |
|---|---|
| `AWS+Command+Line+Interface+Part+1.pdf`, `AWSSlides.pdf` | The course decks |
| `STRESSInstallAndCommands.txt` | Installing `stress` on CentOS / Amazon Linux 2 / Ubuntu + the load-generating command chain |
| `morInEec2_part1.txt` | The manual Apache deploy drill (apt → wget a Tooplate template → unzip → `/var/www/html/` → restart) |
| `multios_websetup.sh` | That same deploy, automated: one script, yum/apt branches, service restart, temp-dir cleanup |

## The stress exercise

`stress` was the tool for watching CloudWatch do its job — burn CPU on a t2.micro and see the alarm fire:

```bash
# install per OS (yum / amazon-linux-extras / apt), then:
stress -c 4 -t 60 && sleep 60 && stress -c 4 -t 60 && sleep 60 && stress -c 4 -t 360 ...
```

The chained command alternates load and rest on purpose, so the CloudWatch graph shows distinct spikes rather than one flat line. This is the same pattern the autoscaling and CloudWatch alarms lessons build on.

## The deploy ladder (where this fits)

`morInEec2_part1.txt` is the fully manual deploy — type every command. `multios_websetup.sh` is step two — script it, detect the OS, loop it. The same idea then climbs:

1. bash script (this repo) → 2. CloudFormation `cfn-init` ([cloudformation-practice](https://github.com/amanvicky/cloudformation-practice)) → 3. Jenkins pipeline to ECS ([Jenkins-CI-CD](https://github.com/amanvicky/Jenkins-CI-CD)) → 4. Kubernetes objects ([kubernetes](https://github.com/amanvicky/kubernetes))

## Commands I actually ran

```bash
aws configure                          # keys + region
aws ec2 run-instances --image-id ami-... --instance-type t2.micro --key-name mykey
aws ec2 describe-instances --query 'Reservations[].Instances[].[InstanceId,State.Name]'
aws ec2 terminate-instances --instance-ids i-...
```

Infrastructure as code starts in [Tf-EC2](https://github.com/amanvicky/Tf-EC2) — 211 dated study cards, one resource each.
