# aws-subnatgwnet

Will create a NAT gateway(s) in an existing VPC 

## Requirements

- AWS credentials and the correct permissions to create the resources
- An existing VPC with a tagged Name
- Existing public subnets that are Tagged with the word `Public` in their name
- Existing private subnets that are Tagged with the word `Private` in their name

## Role Variables

The variables uses in this role are

| Variable Name | Required | Description | 
|----|----|----|
| `region`| **Yes** | The region that you will deploy into |
| `vpc_name` | **Yes** | Used for Rollback purposes |
| `natgw_wait_timeout` | Optional | Period of time to wait for timeout <br> - Default `300` |
| `natgw_wait` | Optional | Wait for subnet to become available <br> - Default `yes` |
| `map_public` | Optional | Assign public IP addresses by default to instances <br> - Default `no` |
| `if_exist_do_not_create` | Optional | Do not create a NAT gateway if one already exists in that subnet <br> - Default `true` |
| `eip_address` | Optional | Elastic IP to attach to the NAT Gateway <br> - Default `empty` - a new Elastic IP will be created |
| `release_eip` | Optional | Relase Elastic IP after NAT Gateway Removal <br> - Default `yes` |


## Dependencies

None

## Example Playbook

### Download dependencies

#### Create requirements file

Create a `requirements.yml` file with the following contents

```
- src: https://github.com/maishsk/aws-natgw
  version: master
```

#### Download dependencies
Run the following command:
```
ansible-galaxy install -r requirements.yml --force -p .
```

### Create playbook
Create a `main.yaml` file with the following contents:
```
---
- name: NAT Gateway Provisioning
  hosts: localhost
  connection: local
  gather_facts: false
  vars_files:
    - vars.yml

  tasks:
  - name: Create Process
    include_role:
      name: "{{ item }}"
    with_items:
      - aws-natgw
    tags: [ 'never', 'create' ]

  - name: Rollback Process
    include_role:
      name: "{{ item }}"
    with_items:
      - aws-natgw
    tags: [ 'never', 'rollback' ]
```

Create a `vars/vars.yml` with the content similar to:
```
vpc_name: maish_test
region: us-east-2
```

## Running the playbook

To create the NAT Gateways

`ansible-playbook main.yml --tags create`

To remove the NAT Gateways

`ansible-playbook main.yml --tags rollback`

## License

BSD

## Author Information
This role was created by [Maish Saidel-Keesing](https://www.maishsk.com/), author of [The Cloud Walkabout](http://cloudwalkabout.com/).