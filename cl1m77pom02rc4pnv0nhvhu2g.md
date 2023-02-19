# Understanding AWS Lambda working behind the scenes

AWS Lambda is a **serverless**, **event-driven** compute service that lets you run code for virtually any type of application or backend service **without provisioning or managing servers**. 

You can trigger Lambda from a range of events and only pay for what you use.

There are multiple use cases of lambda (serverless functions), one such example is 


![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1649127208238/0LH3I2ODN.png)

Check [AWS site](https://aws.amazon.com/lambda/) for more use cases.

Lambda natively supports Java, Go, PowerShell, Node.js, C#, Python, and Ruby code, and provides a Runtime API allowing you to use any additional programming languages to author your functions.

AWS Lambda supports function packaging and deployment as container images.


The Lambda service is split into the 
- control plane 
- data plane

### Control Plane 
The control plane provides the management APIs (for example, CreateFunction, UpdateFunctionCode, PublishLayerVersion, and so on).

### Data Plane
The data plane is Lambda's **Invoke** API that triggers the invocation of Lambda functions.
When a Lambda function is invoked, the data plane allocates an execution environment on an AWS Lambda Worker (or simply Worker, a type of Amazon EC2 instance)


### Lambda execution environments

Lambda will create its execution environments on a fleet of Amazon EC2 instances called AWS **Lambda Workers**. Workers are bare metal EC2 Nitro instances which are launched and managed by Lambda in a separate isolated AWS account which is not visible to customers.

customers cannot directly interact with a worker as it hosted in a network isolated Amazon VPC managed by Lambda in Lambda’s service accounts.


Workers have one or more hardware-virtualized Micro Virtual Machines (MVM) created by Firecracker. Firecracker is an open-source Virtual Machine Monitor (VMM) that uses Linux’s Kernel-based Virtual Machine (KVM) to create and manage MVMs. It is purpose-built for creating and managing secure, multi-tenant container and function-based services that provide serverless operational models.

### Lambda MicroVMs and Workers



![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1649137029361/YR3R8zm1k.png)

Workers have a maximum lease lifetime of 14 hours. When a Worker approaches maximum lease time, no further invocations are routed to it, MVMs are gracefully terminated, and the underlying Worker instance is terminated.

## Firecracker 

Firecracker runs in user space and uses the Linux Kernel-based Virtual Machine (KVM) to create microVMs. 

The fast startup time and low memory overhead of each microVM enables you to pack thousands of microVMs onto the same machine. 

This means that every function, container, or container group can be encapsulated with a virtual machine barrier, enabling workloads from different customers to run on the same machine, without any tradeoffs to security or efficiency. 

Firecracker is an alternative to QEMU , an established VMM with a general purpose and broad feature set that allows it to host a variety of guest operating systems.



![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1649164590623/AFKRgFG5d.png)


### Lambda Invoke Modes
- event mode
- request-response mode

**Event mode** queues the payload for an asynchronous invocation.

**Request-response** mode synchronously invokes the function with the provided payload and returns a response immediately.


![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1649165584211/aqzNR7vPd.png)

Lambda receives a request-response invoke, it is passed to the invoke service directly. If the invoke service is unavailable, callers may temporarily queue the payload client-side to retry the invocation a set number of times. 

If the invoke service receives the payload, the service then attempts to identify an available execution environment for the request, and passes the payload to that execution environment to complete the invocation. 

If no existing or appropriate execution environments exist, one will be dynamically created in response to the request. 

**Event invocation** mode payloads are always queued for processing before invocation. All payloads are queued for processing in an Amazon Simple Queue Service (Amazon SQS) queue.


Queued events are retrieved in batches by **Lambda’s poller fleet**. The poller fleet is a group of EC2 instances whose purpose is to process queued event invocations which have not yet been processed.


When the poller fleet retrieves a queued event that it needs to process, it does so by passing it to the invoke service just like a customer would in a request-response mode invoke.



![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1649166477715/8kfSZL2sF.png)


### Lambda execution environment lifecycle


![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1649165055110/DtR-CPUhr.png)

**Question**: Can you think what happens when there are no lambda execution environment for your function, How much time it takes to spawn the execution environment?


Sources: 
1. [ByteByteGo](https://blog.bytebytego.com/p/how-does-aws-lambda-work-behind-the?s=r) and its references 
2.  [AWS Lambda whitepaper](https://docs.aws.amazon.com/whitepapers/latest/security-overview-aws-lambda/about-aws-lambda.html)