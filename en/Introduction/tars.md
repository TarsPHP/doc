# TARS Features

## Languages agnostic

TARS was developed using C++ initially, and accomplished C++/JAVA/PHP/NodeJS/Go servers and clients through the development.

Tars could also be called as a service leveraging other programming language such as python as a client. 

Services and clients of others languages  including C# is under active development continually.

Different languages achieve basic RPC capabilities through contract agreement with . 

At the same time, TARS will develop different functionality based on different language features.

High performance is the one of the main focus while developing .As for now, high throughput in production environment has been test through,and it aligned with our requirements of high concurrency and high availability.

## Agile development 

First of all, microservices could be built by TARS rapidly.

And  thanks to multi-language support by TARS, it has high compatibility between difference technology stack.

For some scenarios requiring fast iteration, developers can choose PHP to build microservices easily.

And for other scenarios requiring extreme performance, developers build micorservices based on C++.

Last but not least, TARS is equipped with automatic code generation tools for server and client for different language implementation.

Using TARS:

* Service caller can easily generate the calling code according to the protocol and calls remote service like a local function.

* Service providers can quickly generate the skeleton of the code and then develop inside it efficiently.

## High availability

TARS provides a series of basic services to ensure high availability effectively. 

These basic services includes:

* Tars-Registry: Master Control Service,  stores all the available addresses of services centrally, distributed deployment, db storage.
* Tars-Node: Node services,in charge of monitoring all tars services on one machine and resuming  services in time when abnormalities happened;
* Tars-State: Monitoring service,responsible of collecting monitoring information such as the service state, machine state and RPC call information, and noticing  service anomalies in time by setting different alarm policies.

In addition, in order to ensure high availability, each language will cache the available services list for a period of time. If there is some thing wrong within the Tars-Registry, service can fallback to the available services list locally stored.

Some languages ​​have implemented the function that automatically tagging and rejecting a service when its IP is unavailable.

## Highly efficient operations

TARS provides non-destructive options similar to Nginx reload when shipping code.

Due to the different language frameworks, the implementation of non-destructive publishing is also different.

Batches of non-destructive service reboot which ensures services available all the time, is the standard choice for most high availability releases.

In addition, TARS is more powerful than a simple RPC framework because of its operation platform. 

Users can finish these works easily using operation platform:
* View available services IP list.
* Modify service configuration of one machine.
* Service release and version rollback.
* View monitor information reported by RPC calls.
* View monitor information reported by service and machine.
* Configure services single or batch .
