# JQ cheat sheet

## Basic components of jq

To drill down the json document based on a key just select based on a dot and the key. If the key is a list then add square brackets after it. EG to get the load balancer name from this partial list below use the following

    aws elbv2 describe-load-balancers | jq '.LoadBalancers[].LoadBalancerName'

    {
        "LoadBalancers": [
            {
                "LoadBalancerArn": "arn:aws:elasticloadbalancing:eu-west-1:908674476176:loadbalancer/net/cp-certs-gamma-nlb/316f0303b20cee7f",
                "DNSName": "cp-certs-gamma-nlb-316f0303b20cee7f.elb.eu-west-1.amazonaws.com",
                "CanonicalHostedZoneId": "Z2IFOLAFXWLO4F",
                "CreatedTime": "2022-06-22T15:43:42.789000+00:00",
                "LoadBalancerName": "cp-certs-gamma-nlb",
                ....
                ....

The -r flag will give raw output, stripping off quotes.

The pipe symbol works similar to pipe in bash, passing the output of the previous stage and effectively allowing commands to be chained.

## Select based on conditions

## Output formatting

Output can be formatted as comma separated values (@tsv) or tab separated values (@tsv)


Printing field labels

multiple fields

converting to lists


## Copying to linux clipboard, jqplay

The simplest way to test JQ is to put everything in to jqplay and test it there. Within the AWS cli the following will copy output to the clipboard to be pasted into [jqplay](https://jqplay.org)

    aws s3 ls | xclip -sel clip



## Examples


### List all ALBs that are internet-facing

    aws elbv2 describe-load-balancers | jq -r '.LoadBalancers[]|select ((.Type =="application") and (.Scheme == "internet-facing")) .DNSName'


### List ECs2 showing Environment & Name tags, OS and Instance ID as CSV

    aws ec2 describe-instances|jq -r '.Reservations[].Instances[]| ((.Tags // empty) | from_entries) as $tags | [($tags.Environment), ($tags.Name), .PlatformDetails, .InstanceId]|@csv'


### List API Gateways - names and IDs

    aws apigateway get-rest-apis|jq -r '.items[]|"\(.name), \(.id)"'



### List all deployments for a gateway

    aws apigateway get-deployments --rest-api-id gz5nt7u3pb|jq -r '.items[]|"\(.createdDate), \(.id)"'|sort

### Find all rds instances in specific SNG

    aws rds describe-db-instances |jq -r '.DBInstances[]|select (.DBSubnetGroup.DBSubnetGroupName == "SNG NAME")| .DBInstanceIdentifier'

### Elasticache find partial name match

    aws elasticache describe-cache-subnet-groups |jq -r  '.CacheSubnetGroups[]|select( .CacheSubnetGroupName| contains("SNG NAME") )|.CacheSubnetGroupName'

### Get list of all resources from TF state file. [ Got to required level, to_entries puts all keys into an array, select them and just show the key

    cat /tmp/mystate| jq '.modules[].resources | to_entries[] | select(.value.restricted|not) | .key'

### Select data for single instance from large json file:

    aws rds describe-db-instances|jq -r '.DBInstances[] |select (.DBInstanceIdentifier == "rds-INSTANCE-NAME") |    "\(.DBInstanceIdentifier),\(.DBInstanceClass),\(.DBInstanceStatus),\(.AllocatedStorage)"'

### Find KMS aliases for a single key

    AWS_KMS_ALIAS=$(aws kms list-aliases --key-id ${AWS_KMS_KEY_ID}|jq -r ".Aliases[].AliasName"); echo $AWS_KMS_ALIAS



### List of EC2s with private IP set, showing IP and name tag

    aws ec2 describe-instances|jq -r '.Reservations[].Instances[]
    | select (.PrivateIpAddress != null )
    | reduce .Tags[] as $t ( { PrivateIpAddress }; .[$t.Key] = $t.Value )
    | [.PrivateIpAddress, .Name]|@csv'    
