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

**The failing curl** (command and output):

```

```

**The log line that told you what was wrong:**

```

```

**What was wrong, and the fix you applied:**

<!-- One or two sentences. Say what you changed and where you changed it. -->

**The healthy curl after the fix:**

```

```

## 5. Teardown proof

Paste the delete output, or describe the console evidence that the resources are gone.

```

```
