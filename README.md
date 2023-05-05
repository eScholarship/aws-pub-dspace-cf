# aws-pub-dspace-cf (DRAFT)

A collection of Cloudformation templates for use in provisioning a DSpace 7 front-end and back-end in AWS.

> **Warning**
> **this is a work in progress, has not been tested, and most likely does not
> work.**

[![Lint
Status](https://github.com/eScholarship/aws-pub-dspace-cf/workflows/Lint/badge.svg)](https://github.com/eScholarship/aws-pub-dspace-cf/actions?query=workflow%3ALint)


## Prerequisits

You will need the following resources before you create a stack based on the
load balancer template:

* Elastic IP (EIP) for the load balancer/reverse proxy
* SSL Certificate ARN for the load balancer/reverse proxy

### Creating an EIP:

```
aws ec2 allocate-address --domain vpc --region <region> --output text
```
Replace <region> with the AWS region you want to create the EIP in.

This will return the EIP address that you can use when creating the load balancer.

### Creating an SSL Certificate:

```
aws acm request-certificate --domain-name <domain-name> --validation-method DNS --region <region> --output text
```
Replace <domain-name> with the domain name you want to use for the SSL certificate, and <region> with the AWS region you want to create the certificate in.

This will return the ARN of the certificate, which you can use when creating the load balancer.

## Usage

Before you begin, be sure you have already completed the steps in the prerequisites section above. 

### Create the Security Groups
```
aws cloudformation create-stack --stack-name DSpaceSecurityGroups --template-body
file://dspace-sg.yaml --parameters #TODO
```

### Create the DSpace API EC2
```
aws cloudformation create-stack --stack-name DSpaceAPI --template-body
file://dspace-api-ec2.yaml --parameters #TODO
```

### Create the DSpace Client EC2
```
aws cloudformation create-stack --stack-name DSpaceClient --template-body
file://dspace-client-ec2.yaml --parameters #TODO
```

### Create the Load Balancer
```aws cloudformation create-stack --stack-name DSpaceLoadBalancer --template-body
file://dspace-alb.yaml --parameters
ParameterKey=CertificateArn,ParameterValue=<certificate-arn> --region
<region>```

Replace <eip-address> with the EIP address you created earlier, <certificate-arn> with the ARN of the SSL certificate, and <region> with the AWS region you want to create the stack in.

### Associate the EIP with the Load Balancer
After the load balancer has been created, you need to associate the EIP with the load balancer. You can do this with the AWS CLI:
```aws elbv2 associate-load-balancer-with-ip-address --load-balancer-arn <load_balancer_arn> --ip-address <eip_allocation_id>
```

## Contributing

1. Fork it!
2. Create your feature branch: `git checkout -b my-new-feature`
3. Commit your changes: `git commit -am 'Add some feature'`
4. Push to the branch: `git push origin my-new-feature`
5. Submit a pull request :D

## History

TODO: Write history

## Credits

TODO: Write credits

## License

TODO: Write license
