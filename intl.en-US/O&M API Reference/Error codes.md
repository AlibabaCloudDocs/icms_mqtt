# Error codes {#concept_109365_zh .concept}

If the MQ for MQTT API is called successfully, ResponseCode=200 is returned to the MQTT client. If the call fails, error codes and descriptions are returned. You can find a solution based on the following error messages.

|Error code|Symptom and cause|Solution|
|----------|-----------------|--------|
|ONS\_SYSTEM\_ERROR|The backend of RocketMQ is abnormal.|Contact Alibaba Cloud Customer Services by submitting a ticket.|
|ONS\_SERVICE\_UNSUPPORTED|The call is not currently supported in the corresponding region.|Contact Alibaba Cloud Customer Services to check whether the operation is available.|
|ONS\_INVOKE\_ERROR|API call failed.|Contact Alibaba Cloud Customer Services.|
|BIZ\_FIELD\_CHECK\_INVALID|Parameter verification failed.|Check whether the parameters are valid based on the API reference.|
|BIZ\_TOPIC\_NOT\_FOUND|The topic cannot be found.|Check whether the entered topic is valid or available.|
|BIZ\_SUBSCRIPTION\_NOT\_FOUND|The target subscription group ID cannot be found.|Check whether the group ID exists or whether the filter is correct.|
|BIZ\_PUBLISHER\_EXISTED|The specified group ID already exists.|Try again with a new group ID.|
|BIZ\_SUBSCRIPTION\_EXISTED|The specified group ID already exists.|Try again with a new group ID.|
|BIZ\_CONSUMER\_NOT\_ONLINE|The client with the specified group ID is offline.|Ensure that the consumer client exists and then try again.|
|BIZ\_NO\_MESSAGE|No message matches the filter.|Check the filter and check whether messages have been published in the query range.|
|BIZ\_REGION\_NOT\_FOUND|The region where you request the service cannot be found.|Check whether the parameters for the region where you request the service are valid.|
|BIZ\_TOPIC\_EXISTED|The specified topic already exists.|Try again with a new topic name.|
|BIZ\_PUBLISH\_INFO\_NOT\_FOUND|The requested group ID cannot be found.|Check whether the group ID exists or whether the request condition is correct.|
|EMPOWER\_EXIST\_ERROR|The current authorization already exists.|Verify the request parameters and try again or query the authorization.|
|EMPOWER\_OWNER\_CHECK\_ERROR|The current user is not the owner of the topic to be authorized.|Check the ownership of the resource.|

