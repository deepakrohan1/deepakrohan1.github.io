---
layout: post
title:  "Getting Started On Terraform"
date:   2022-02-11 05:40:34 -0400
categories: Terraform Getting Started
---


Lets get started to learn terraform basics

# Pre-requisites
* Any cloud provider account (AWS will be used)
* terraform installed 

# Installation

## On mac

Since I kind of use mac for my development work you can install it using brew

```
brew install terraform 

brew update terraform

```

# Getting Started

## Initialize a directory

Just create an empty directory on your system and run terraform command 

```
mkdir tf
cd tf
terraform init
```

## Sample file to get Started

You can create a s3 bucket with below configuration. You can add more properties inside

```
resource "aws_s3_bucket" "buck" {
    bucket = "name-of-bucket"
}
```

## Using data

## Creating and Using variables

You can create and use variables in differe ways. 

### 1. In Terraform File

You can write the variables in the terraform file and access them inside by using below method

```
variable "bucket_name" {
    default = "random-bucket-name"
    description = "Name of the bucket to create" // for people to understand
}

// create resource s3 bucket
resource "aws_s3_bucket" "bucket" {
    bucket = var.bucket_name
    force_destroy = true
}
```

### 2. Use the tfvars file

You can create a vars file and use them `tfvars` This can be done for each environment and passed in as an argument for the `apply or plan` command

Assuming a file with the name dev.tfvars is created

```
terraform plan --var-file="dev.tfvars"
```

### 3. Pass it as argument

You can also pass it as an argument when you run in commandline

```
terraform apply -var="bucket=some-name"
```

## locals

Kinda of like a dictionary data type which can be used for variables 

```

//create ec2 instance
resource "aws_instance" "instance" {
    ami = "ami-0f69c8ae"
    instance_type = local.instance_type
    tags = {
        Name = local.instance_name
    }
}

locals {
  instance_name = "test-instance"
    instance_type = "t2.micro"
}
```

## Interpolation of Variables

You can combine var and string using a `${}` 

```
name = "${bucket_name}-dev"
// You can do ternary operators
name = bucket_name == '' ? "some-def-name" : ${bucket_name}
```