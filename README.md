4 Nodes connected to Zookeeper and a leader is elected
![image 1](https://github.com/jtn-b/leader-election/blob/master/src/main/resources/leader-election.png)

<br>


Current leader dies, new leader elected
![image 2](https://github.com/jtn-b/leader-election/blob/master/src/main/resources/leader-dies.png)
<br>
<br>


A worker node dies, it's successor registers new watcher
![image 3](https://github.com/jtn-b/leader-election/blob/master/src/main/resources/worker-dies.png)
<br>


# Apache Zookeeper



Apache ZooKeeper is an open-source server for highly reliable distributed coordination of cloud applications. It is a project of the Apache Software Foundation.



## Zookeeper Client Threading Model

- Main Thread -> Runs the main function
- I/O Thread and Event Thread are created after creation of the Zookeeper object
- I/O thread handles all network communication *(pings,req,res,timeouts)*
- Even thread handles events *(connected,disconnected,watchers, triggers)*

## Watchers and Triggers

Class must implement Watchable interface and override process() to handle events. Watchers for methods like getChildren() and getData() need resubscription after an event.


# The Algorithm

1. All nodes create z_nodes under '/election' parent with the Zookeeper server.
2. Each new client checks for all the existing z_nodes using getChildren() method
3. If it is the smallest in the sequence, it is the leader , else it registers watcher for its predecessor.
4. If the watcher for predecessor produces a NodeDeleted event, re-election method is called.

