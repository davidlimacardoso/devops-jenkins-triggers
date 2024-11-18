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


# Make a Jenkins Job Trigger

## Create a Token Name
Inside `JOB_PIPELINE > Build Triggers > Trigger builds remotely > ADD_TOKEN_NAME` 

Below field is displayed JENKINS_URL/job/Build/build?token=TOKEN_NAME or /buildWithParameters?token=TOKEN_NAME... Set your Jenkins host and token name.

eg: 
http://localhost:8080/job/Build/build?token=mybuildtoken


## Generate the user API Token
Generate the user API Token to user at CRUMB to in `Your Profile > Security > Add new Token (Genenate)`

eg: admin:11b084d3d37eef1194a9306b4fecce93dc

## CRUMB

After to get the results commands build the command below:

```bash
wget -q --auth-no-challenge --user admin --password 123456 --output-document - 'http://localhost:8080/crumbIssuer/api/xml?xpath=concat(//crumbRequestField,":",//crumb)'
```
Output result:
`  Jenkins-Crumb:1833384d9228b0c09a67353e9c1893bd50351977901fb1d76faadb80c2122161
`

With the Jenkins CRUMB will use to trigger the Build Pipeline with webhook: 

```bash
curl -I -X POST http://admin:11b084d3d37eef1194a9306b4fecce93dc@localhost:8080/job/Build/build?token=mybuildtoken -H "Jenkins-Crumb:1833314d9228b0c09a67353e9c1863bd50351977901fb1d76faadb80c2122161" 
```
