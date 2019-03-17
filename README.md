# aws-subnatgwnet

Will create a NAT gateway(s) in an existing VPC 

## Requirements

An existing VPC.
Existing public Subnet(s)

## Role Variables

The variables uses in this role are

| Variable Name | Required | Description | 
|----|----|----|
| `region`| **Yes** | The region that you will deploy into |
| `public_subnet_ids` | **Yes** | List of Public subnets for Nat Gateways <br> - This can either be explicitly declared or passed down from a previous playbook/role |
| `vpc_id` | Optional | The ID of the VPC | 
| `vpc_name` | Optional | Used for Rollback purposes |
| `natgw_wait_timeout` | Optional | Period of time to wait for timeout <br> - Default `300` |
| `natgw_wait` | Optional | Wait for subnet to become available <br> - Default `yes` |
| `map_public` | Optional | Assign public IP addresses by default to instances <br> - Default `no` |
| `if_exist_do_not_create` | Optional | Do not create a NAT gateway if one already exists in that subnet <br> - Default `true` |
| `eip_address` | Optional | Elastic IP to attach to the NAT Gateway <br> - Default `empty` - a new Elastic IP will be created |
| `release_eip` | Optional | Relase Elastic IP after NAT Gateway Removal <br> - Default `yes` |


## Dependencies

None

## Example Playbook

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

```
---
- name: Create NAT Gateway
  hosts: localhost
  connection: local
  gather_facts: false
  vars_files:
    - vars/vars.yml
  roles:
    - aws-natgw
```

And `vars/vars.yml` contains

```
vpc_name: maish_test
region: us-east-2
vpc_id: vpc-077c143fcf318b68c
public_subnet_ids:
  - subnet-083752f6318c348e0
  - subnet-0ad42b093bd8c9390
```

## Running the playbook

To create the VPC

`ansible-playbook main.yml -e "create=true"`

To remove the VPC

`ansible-playbook main.yml -e "rollback=true"`

## License

BSD

## Author Information
This role was created by [Maish Saidel-Keesing](https://www.maishsk.com/), author of [The Cloud Walkabout](http://cloudwalkabout.com/).