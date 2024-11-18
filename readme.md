## AWS Prefix Listo to Security Group
Add the externals IPs from github hooks to prefix list security group:

```bash
# List github hooks ips  
curl -s https://api.github.com/meta | jq '.hooks'
```
## Create prefix list
```bash
aws ec2 create-managed-prefix-list \
    --prefix-list-name github_hooks_ip_list \
    --max-entries 5 \
    --address-family <IPv4|IPv6> \
    --tag-specifications 'ResourceType=prefix-list,Tags=[{Key=key1,Value=value1},{Key=key2,Value=value2}]' \
    --prefix-list-entries '[{"Cidr": "192.0.2.0/24", "Description": "Github hooks ip"},{"Cidr": "192.0.3.0/24", "Description": "Github hooks ip"}]'
```
After to attached in Securit Group of Instance.
