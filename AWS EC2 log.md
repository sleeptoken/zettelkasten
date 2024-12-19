
2024-12-19 11:40

Source: #AOC24 
### AWS CloudWatch

 - AWS CloudWatch is a monitoring and observability platform that gives us greater insight into our AWS environment by monitoring applications at multiple levels
###  AWS CloudTrail

- CloudWatch can track infrastructure and application performance, to monitor actions in your AWS environment? you gotta use AWS CloudTrail.

Actions can be those taken by a user, a role (granted to a user giving them certain permissions) or an AWS service and are recorded as events in AWS CloudTrail.
Essentially, any action the user takes (via the management console or AWS CLI) or service will be captured and stored.
### JQ

Transform and filter Cloudtrail logs (JSON-formatted) into meaningful data we can understand and use to gain security insights.
JQ takes two inputs: the filter you want to use, followed by the input file.
We start our JQ filter with a `.` which just tells JQ we are accessing the current input. From here, we want to access the array of values stored in our JSON (with the `[]`). Making our filter a `.[]`

if we wanna filter the book titles then use command 
`jq '.[] | .book_title' book_list.json

commands for taking a look at aws logs using jq looks like 
```sh
jq -r '["Event_Time", "Event_type", "Event_Name", "User_Name", "Source_IP", "User_Agent"],(.Records[] | select(.userIdentity.userName == "glitch") | [.eventTime,.eventType, .eventName, .userIdentity.userName //"N/A",.sourceIPAddress //"N/A", .userAgent //"N/A"]) | @tsv' cloudtrail_log.json | column -t -s $'\t'
```
- The **-r** flag tells **jq** to output the results in RAW format instead of JSON.`






### References
Day 7


