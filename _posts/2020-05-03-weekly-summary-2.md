---
title: "Weekly Summary - What I Learned"
date: 2020-05-03 00:18:00 +0900
categories: dev
comments: true
---

During this first quarter, I had a long journey of learning Infrastucture as Code. I started with Terraform, creating resources and rules over various Clouds such as Azure, AWS, GCP and other local providers such as Conoha, and SakuraVPS.

(I will summarize my personal experiences only.)

## Terraform

My first step to 'Infrastructure as Code' was with [Terraform][terraform]. As we deal with hundreds of customers' environment over dozens of different cloud providers, managing every cloud platform accounts and log trace of every changes have been great challenge.

Since January, I was in charge of temporary Task-force team to create Terraform configuration presets for each cloud providers. Usually our web applications are consisted of 1 or more compute instances with Load Balancer(Both L4 or L7), therefore I wanted to deploy newer customers' environment with preset.

 While I made preset, I tried to use seperate configuration for credentials (like API Keys) and load it from more secure directory where users in SSH cannot read it easily.

While I stepped on, I got used to every cloud providers' distinct structures and different services, such as Application Load Balancer in Azure and ELB in AWS. It helped me to understand each philosophy and ease of use they are looking forward to.

## Ansible

After deploying environment with Terraform, I used [Ansible][ansible] script to execute shell commands to update environment and initialize KUSANAGI with no delay.

Writing playbooks were not that difficult, but secure access and credential management was another big issue. I decided to use AWX as a control tower, and store every credentials in that private server. Docker image came very handy to run AWX.

Sincerely, we already have other tools such as Zabbix, Kibana which tries to manage instances' status so I didn't dive further such as Administration over Instance Groups, but I'm looking forward to found out better options for administration of IaC.

## Yaml and Json

Terraform is famous for intense use of yaml, but since v0.12, Terraform provides both yaml and [json][json] configuration in full scale. Just for experimental reason, I seperated credential(API Key) configuration with Json, and made automated script to recreate them whenever Terraform command is executed through customized KUSANAGI shell command. In this way, we won't let stale configuration files with API keys included.

## Building rpm package in Ubuntu with Docker

Since I started working at home, I made my development environment with WSL Ubuntu. It is not THAT fast, but I was very anticipating further steps Windows take with Linux Kernal. (Waiting for official WSL2 Release...)

The problem of current WSL is the lack of RedHat or centOS environment, which  KUSANAGI is consist of. After adding modsecurity addon to nginx, I had to make newer package file in rpm, so I decided to use centOS Docker image to create rpm build environment.

While I run a docker image, I mount directory with rpm build necessaries in, and the result of built rpm file is added into same directory. It's a simple way to use docker, but very effective way to manage virtual resource which won't be used later on. (such as, vagrant)

[terraform]: https://www.terraform.io/docs/cli-index.html
[ansible]: https://www.ansible.com/
[json]: https://www.terraform.io/docs/configuration/syntax-json.html
