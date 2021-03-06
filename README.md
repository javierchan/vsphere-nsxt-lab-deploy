# vsphere-nsxt-lab-deploy

## Table of Contents

* [Description](#Description)
* [Changelog](#Changelog)
* [Requirements](#Requirements)
* [Usage](#Usage)
* [Compatibility](#Compatibility)
* [The deployment](#The-deployment)
* [Diagrams](#Diagrams)
* [Development](#Development)
* [Credits](#Credits)

## Description

This repository contains a set of Ansible Playbooks that deploy and configure vCenter, nested ESXi, NSX-T Manager, and NSX-T Edge nodes. The primary use case is speedy provisioning of a consistent nested vSphere/NSX-T lab environment.

## Changelog

See [CHANGELOG.md](CHANGELOG.md)

## Requirements

* A physical standalone ESXi host
* An Ubuntu 18.04/20.04 VM with the following packages:
  * apt install python3 python3-pip xorriso
  * pip3 install ansible pyvim pyvmomi
  * git clone https://github.com/rutgerblom/vsphere-nsxt-lab-deploy.git
* ESXi and VCSA ISO files as well as the NSX-T Manager OVA file
* If deploying NSX-T you'll need an NSX-T license (Check out [VMUG Advantage](https://www.vmug.com/membership/vmug-advantage-membership) or the [NSX-T Product Evaluation Center](https://my.vmware.com/web/vmware/evalcenter?p=nsx-t-eval))

## Usage

Rename **answerfile_sample.yml** to **answerfile.yml** and modify the settings according to your needs. 

Start the deployment with: **ansible-playbook deploy.yml**

Remove the deployment with: **ansible-playbook undeploy.yml**

## Compatibility

The following versions of vSphere and NSX-T can be deployed:
* ESXi version 6.7 and 7.0
* vCenter version 6.7 and 7.0
* NSX-T version 2.5 and 3.0

## The deployment

Using the default **deploy.yml** the following is deployed:
1. vSwitch and port groups on the physical ESXi host
1. VyOS router
1. vCenter Sever Appliance
1. 6 ESXi VMs
1. Configuration of the nested vSphere environment:
   * ESXi hosts
   * Distributed switch
   * vSphere clusters "Compute-A" and "Edge"
   * vSAN
1. NSX-T:
   * NSX Manager
   * vCenter Compute Manager (the deployed vCenter Server Appliance)
   * Transport Zones
   * IP pool for TEP
   * Uplink Profiles
   * Transport Node Profile
   * Two NSX-T Edge Transport Nodes
   * Edge Node Cluster
   * ESXi Transport Nodes
   * Tier-0 Gateway (NSX-T 3.0 only)
   * BGP peering with VyOS router (NSX-T 3.0 only))

Ansible play recap from 05/05/2020:

![](images/play-recap.png)

### Diagrams

The following diagrams show what is deployed when using the default settings in **answerfile.yml**

A diagram of the physical environment.
![Physicaloverview](images/vsphere-nsxt-deploy-phys.png)

A diagram of the nested vSphere environment.
![Logicaloverview](images/vsphere-nsxt-deploy-log.png)

A diagram of the NSX-T logical network (provisioned with NSX-T 3.0 only).
![Logicalnsxoverview](images/vsphere-nsxt-deploy-nsx.png)

## Development

* TODO: Add an option to deploy against vCenter
* TODO: Improve NSX-T Edge VM deployment
* TODO: Add more Playbooks for NSX-T logical networking

## Credits

A big thank you to **Yasen Simeonov**. His project at https://github.com/yasensim/vsphere-lab-deploy was the inspiration for this project. Another big thank you to **Luis Chanu (VCDX #246)** for helping me push this project forward all the time. And thank you **vCommunity** for trying this out and providing feedback.
