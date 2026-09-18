# Deployment Evidence

Fill this in as you go. Paste real output, not descriptions of output. A TA reads this
file with you at recitation.

## 1. Deployed URL and instance id

| Property    | DescribeStacks |
| ----------- | ----------- |
| InstanceId  |  i-0a206bc61e704cb62 |
| ServiceUrl  | http://ec2-54-91-57-171.compute-1.amazonaws.com:8080 |



## 2. External health check

Run the check from your own machine, not from the instance. Paste the command and the
response.

```
Sanjana@sanjanas-mac 17514-f26-lab04 % curl http://ec2-54-91-57-171.compute-1.amazonaws.com:8080/api/health
{"status":"ok"}%    
```

## 3. What the template created

Three or four sentences, your own words. What compute, what network access, and what
glue made the service start.

The template created an EC2 instance on AWS that hosts the API. We made an HTTP request to the URL that 
serves the API and we checked the `/api/health` endpoint.

## 4. Scenario 2 diagnosis

### Deployed URL and instance id

| Property    | DescribeStacks |
| ----------- | ----------- |
| InstanceId  |  i-00dba05d37e3a365a |
| ServiceUrl  | http://ec2-100-56-229-120.compute-1.amazonaws.com:8080 |


**The failing curl** (command and output):

```
Sanjana@sanjanas-mac 17514-f26-lab04 % curl http://ec2-100-56-229-120.compute-1.amazonaws.com:8080/api/health
curl: (7) Failed to connect to ec2-100-56-229-120.compute-1.amazonaws.com port 8080 after 22 ms: Couldn't connect to server

```

**The log line that told you what was wrong:**

From within the EC2 container:

```
sh-5.2$ sudo docker ps
CONTAINER ID   IMAGE                                     COMMAND                  CREATED         STATUS         PORTS                                       NAMES
e1b98c197d45   ghcr.io/cmu-17-214/lab04-service:latest   "/__cacert_entrypoin…"   2 minutes ago   Up 2 minutes   0.0.0.0:8080->8080/tcp, :::8080->8080/tcp   lab04-service
sh-5.2$ sudo docker logs lab04-service
lab04-service listening on 9090
```

**What was wrong, and the fix you applied:**

<!-- One or two sentences. Say what you changed and where you changed it. -->
The port forwarding maps host port 8080 to container port 8080. But, when we start up the deployment, the port forwarding environment variable
is overridden to port 9090 on the container. So, unless we are now trying to reach the service at container's 9090, we can't access
the service.

The fix was to remove the port forwarding override.

**The healthy curl after the fix:**

```
Sanjana@sanjanas-mac 17514-f26-lab04 % curl http://ec2-54-205-185-242.compute-1.amazonaws.com:8080/api/health
{"status":"ok"}% 
```

## 5. Teardown proof

Paste the delete output, or describe the console evidence that the resources are gone.

```
Sanjana@sanjanas-mac 17514-f26-lab04 % aws cloudformation delete-stack --stack-name lab04-service
aws cloudformation wait stack-delete-complete --stack-name lab04-service
Sanjana@sanjanas-mac 17514-f26-lab04 % aws cloudformation describe-stacks --stack-name lab04-service

aws: [ERROR]: An error occurred (ValidationError) when calling the DescribeStacks operation: Stack with id lab04-service does not exist
```
