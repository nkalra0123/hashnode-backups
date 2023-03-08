---
title: "jq Command for JSON Processing"
seoDescription: "jq command for extracting fields from json, we learn how to install jq and learn how to use jq commands on a json file"
datePublished: Wed Mar 08 2023 12:05:52 GMT+0000 (Coordinated Universal Time)
cuid: clezmvuka000h08l2701pfziy
slug: jq-command-for-json-processing
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/xbEVM6oJ1Fs/upload/5a4ba22ac7ff42eb309b797b3e668b26.jpeg
tags: linux, command-line, jq, scripting

---

JSON (JavaScript Object Notation) is a widely used format for exchanging data between systems. It is human-readable and easy to parse, making it a popular choice for APIs, configuration files, and data storage. In Linux, the jq command is a powerful tool for processing JSON data. In this blog post, we will explore how to use the jq command for JSON processing.

## Installation

jq is not typically installed by default on Linux systems. To install it, you can use your distribution's package manager. For example, on Debian-based systems, you can run the following command:

```bash
sudo apt-get install jq

or 

brew install jq
```

## Basic Usage

The jq command reads JSON data from standard input or a file and outputs formatted JSON or text. Here are some basic examples of how to use jq:

Let's say we have this JSON file  
{ "users": \[ { "first": "Stevie", "last": "Wonder" }, { "first": "Michael", "last": "Jackson" } \] }

1. Print the contents of a JSON file:
    
    ```bash
    jq '.' users.json
    ```
    
    1. Select a specific key from a JSON file:
        
    
    ```bash
    jq '.users' users.json
    ```
    
    1. Select multiple keys from a JSON file:
        
    
    ```bash
    jq '.users[] | {key1: .first, last: .last}'  users.json
    ```
    
    1. Filter JSON data based on a condition:
        
    
    ```bash
    jq '.users[] | select(.first == "Stevie")' users.json
    ```
    
    We can add "and", "or" conditions in the select statement
    
    ```bash
    select(.role=="assistant" or .role=="user" or .content_type=="text")
    ```
    
    If after adding some select condition, we are getting null values, we can ignore them by adding "select( . != null )"
    
    ```bash
    | select(.role=="assistant" or .role=="user" or .content_type=="text") | select( . != null )
    ```
    
    jq command is a powerful tool for processing JSON data on Linux/mac systems. With its many features and options, it can handle a wide range of JSON processing tasks. By mastering jq, you can streamline your JSON workflows and save time and effort.
    

If you liked this blog, [**you can follow me on twitter**](https://twitter.com/nkalra0123), and learn something new with me.