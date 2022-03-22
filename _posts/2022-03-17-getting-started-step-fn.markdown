---
layout: post
title:  "Step Function Phase 1"
date:   2022-17-03 12:40:34 -0400
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