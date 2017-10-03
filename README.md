# role-and-permission
Setting up roles and permissions for a system on graph based architecture

## About
> This document will try to address problem of role and permissions upon any arbitrary relations between the entities in a system, using a graph based architecture.

## Fundamental rules
A operation usually have the following three properties
* The resource (In case it is a table, eg when resource is to be created, the table itself is resource that keep a list of resources), having two properties
  * Table name
  * Resource Id (if already exists)
* How to reach that resource,
  * That is the path of traversal about how to reach that resource
  * Eg taking the url path like
    * /projectId/containerId/reqId
* And the Operation
  * Eg RW

## An example scenario
> Now the example scenario (our problem may relate with this), (in my opinion the graph can represent any kind of scenario) Say we have following relationship, represented as graph. (resource as nodes, and to reach a particular resource, the visitor always need to traverse to a set of nodes in graph)
> <readPowerOn, writePowerOn> are the meta deta written on top of a node in the graph

![Graph based role and access scenario](https://github.com/codeofnode/graph-role-permission/raw/master/example.png)

Now to access a resource, There are 4 traversal paths

* /p1/c1/r1
* /p1/c2/r1
* /p2/c2/r1
* /w1/r1

Say as an example, U1 tries to write a requirement (resource)

The UI (client script) need to decide, in whichever order that might be suited, while performing an operation which path is best suited.
And hence client will sent the operation request to server with the best suited traversal path.

Traversal path once received, the system will find the following


Power of U1, say user has the power of say 6 in the system


* Path1 => p1(not allowed as 6(user power) < 8(rightPowerOn of P1)), request denied, 403
* Path2 => p1(not allowed), request denied, 403
* Path3 => p2(allowed as 6(user power) > 5(rightPowerOn of P2)), -> c2(not allowed), request denied 403
* Path4 => w1 (allowed  as 6(user power) > 4(rightPowerOn of w1)) -> r1(allowed (6>5)), OPERATION ALLOWED.

So client should choose path4 (client should know from where user has reached r1, based on page, or history or xyz factors)
