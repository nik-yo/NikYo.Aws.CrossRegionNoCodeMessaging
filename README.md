# Cross Region No Code Messaging

I first encounter this kind of use case when I needed to route notification from AWS Marketplace to a VPC in different region and different AWS account. And the source SNS only allows subscription from a particular AWS account. I was looking for the lowest maintenance and cost effective solution.

## Notes

Here are the resources on source region. In my actual use case, these resources actually reside in a different AWS account, and the SNS are managed by AWS, so I don't have control over the policy. But in this case, we will just use same AWS account:

SNS (strict) -> SQS -> Event Bridge Pipe -> SNS -> (destination region)

And here are the resources on a destination region which is different from the source region in my actual use case. The target was actually a lambda function with connection to VPC, but I don't include the lambda function here:

SQS (subscribed to SNS in source region) -> (target, e.g. Lambda function)

## Usage

1. Create a stack in source region using source-region.yaml template. Note the to-destination-sns topic ARN, it can be found under stack output.

2. Create a stack in destination region using destination-region.yaml template. Pass the topic ARN from step 1 and source region to the parameters.

3. Publish a message in source-sns topic in source destination and check if the message is received by destination SQS.

## Cost

Cost should be minimal, but please delete the resources to prevent accidental execution.