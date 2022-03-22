---
layout: post
title:  "Step Function Phase 1"
date:   2022-03-17 12:40:34 -0400
categories: Step Function
---

A step function is constructed using the AWS State Language
* JSON
* DSL
* Structure

```json
{gs
"Comment": "A hello world application",
"StartAt": "HelloWorld",
"States": {
    "HelloWorld": {
        "Type": "Pass",
        "Result": "Hello World",
        "End": true
        }
    }
}
```




# Different Types 
* Pass - _Used to debug stuff pass from one state to other_
* Wait - _Wait is used to wait for a duration example __Seconds:10__ would wait for 10sec_
* Succeed
* Fail - _Would stop execution_
* Choice - _Conditional Branching logic if or switch case_
* Parallel
* Task (Typically Lambda but can be any AWS Resource) _Running any task_

## 1.1 Task State
Used for invoking a task 

```json
"TaskState": {
    "Type": "Task",
    "Resource": "<ARN TO LAMBDA>",
    "TimeoutSeconds": 300,
    "HeartbeatSeconds": 60,
    "Next": "NextState"
}
```

## 1.2 Data Processing

### InputPath
Select a piece of the state input to pass for data processing

Only a portion of the input passed

### ResultPath
Take the results of executing the states task and places them on input json kind of like a overlay

Task Result is sent as output

### OutputPath
Selects portion of input  to send as output

Portion of the  output to the next state

## Invoking step function from Lambda

For invoking step function we need to import the boto3 library and get the reference to step function client

```python
client = boto3.client('stepfunctions')
```
and you can invoke it using the `start_execution` on the client 
```python
def lambda_handler(context, event):
   response = client.start_execution(
        stateMachineArn='<ARN_FOR_STATE_MACHINE>',
        name="uniq_name",
        input='{}'
        )
```

## 1.4 Wait State

A wait can be added to the step function, can also pass variables reference to the
`"$.waitseconds" "$.expirydate"`

```json
"wait_for_ten_seconds": {
    "Type": "Wait",
    "Seconds" : 10,
    "Next": "EndState"
}

"wait_for_ten_seconds": {
    "Type": "Wait",
    "Timestamp" : "2000-01-01T12:00:22:Z",
    "Next": "EndState"
}
```

## 1.5 Choice States
Can be used to add if else statement or switch case statement

```json
"ValState": {
    "Type": "Choice",
    "Choices": [
        {
            "Variable": "$.sum",
            "NumbericGreaterThan": 10,
            "NextState": "GtThan10State"
        },
    ]
        "Default": "DefaultState"
}
```

### Supported Types
* boolean
* numeric
* String
* Timestamp

### Comparision Operators
* Equals
* GreaterThan
* GreaterThanEquals
* LessThan
* LessThanEquals

`Not, And and Or`

## 1.6 Error Handling

You can handle errors in state functions and give it number of retry attempts along with BackOffRate

Simple Example

```json
"MySimpleTask": {
    "Type": "Task",
    "Resource": "<ARN>",
    "Retry": [
        {
            "ErrorEquals": ["CustomError", "SomeOtherError"],
            "IntervalSeconds": 1,
            "MaxAttempts": 5,
            "BackoffRate": 2.0
        },
    ],
    "Catch": [
        {
            "ErrorEquals": ["SomeExceptions"],
            "Next": "CustomErrorFallback
        }
    ]
}
```
Catch block runs if there are only catch blocks present with exception or if the retry block has still completed with exceptions


## Buying a products Lambda


```python
import json
import logging
from aws_lambda_powertools.event_handler import APIGatewayRestResolver
import boto3
from datetime import datetime

client = boto3.client("stepfunctions")
app = APIGatewayRestResolver()
logging.basicConfig(level=logging.INFO)


@app.route("/products", method=["POST"])
def create_product():
    products = app.current_event.json_body
   

    client.start_execution(
        stateMachineArn='<STEP_FN_ARN>',
        name='ASLExecution' + datetime.now().strftime("%Y%m%d%H%M%S"),
        input=json.dumps({"products": products})
    )
    return {"products": products}
```

## Step Function to process each incoming item from user

```python
def lambda_handler(event, context):
 # get queue of products from event
    products_queue = event["products"] 

 # pop the first product
    product = products_queue.pop(0)
    print("product: ", product)

 # make a post request to the api with the product
    response = requests.post("<API>", json=product)

    data = response.json()
    print("response: ", data)
 # check to see if error response
    if response.status_code != 200:
        return response.text

 # return the response
    return {
        "remainder": {
            "products": products_queue,
            "products_count": len(products_queue),
            # get id from resoponse
            "id": response.json()["body"]["id"]
        }
    }
```

## Working with the status of your product

```python
def lambda_handler(event, context):
    products_left = event['products']
    id = event['response']

    # send the request out to api for status
    response = requests.get(
        "<API>/{id}",
    )

    # if the status is 200 or 201
    if response.status_code == 200:
        data = json.loads(response.json()["body"])
        return data["complete"]

    else:
        raise Exception("Something went wrong")
```

