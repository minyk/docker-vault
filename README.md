Vault Cluster for Test
======================

Zookeeper backed Vault cluster for test. All data is not persistent, so if you down the cluster, it's down.

# Startup
Start zookeepers first:
```
$ docker-compose up -d zoo1 zoo2 zoo3
```

Check zookeeper logs:
```
$ docker-compose logs zoo1 zoo2 zoo3
zoo1_1   | 2018-05-09 04:35:44,986 [myid:1] - INFO  [QuorumPeer[myid=1]/0.0.0.0:2181:Follower@65] - FOLLOWING - LEADER ELECTION TOOK - 262
zoo1_1   | 2018-05-09 04:35:44,988 [myid:1] - INFO  [QuorumPeer[myid=1]/0.0.0.0:2181:QuorumPeer$QuorumServer@184] - Resolved hostname: zoo3 to address: zoo3/172.18.0.2
zoo3_1   | 2018-05-09 04:35:44,994 [myid:3] - INFO  [LearnerHandler-/172.18.0.4:38422:LearnerHandler@346] - Follower sid: 2 : info : org.apache.zookeeper.server.quorum.QuorumPeer$QuorumServer@4a2a9240
zoo3_1   | 2018-05-09 04:35:44,995 [myid:3] - INFO  [LearnerHandler-/172.18.0.3:50266:LearnerHandler@346] - Follower sid: 1 : info : org.apache.zookeeper.server.quorum.QuorumPeer$QuorumServer@6f6367bd
zoo3_1   | 2018-05-09 04:35:45,003 [myid:3] - INFO  [LearnerHandler-/172.18.0.3:50266:LearnerHandler@401] - Synchronizing with Follower sid: 1 maxCommittedLog=0x0 minCommittedLog=0x0 peerLastZxid=0x0
zoo3_1   | 2018-05-09 04:35:45,003 [myid:3] - INFO  [LearnerHandler-/172.18.0.3:50266:LearnerHandler@410] - leader and follower are in sync, zxid=0x0
zoo3_1   | 2018-05-09 04:35:45,003 [myid:3] - INFO  [LearnerHandler-/172.18.0.3:50266:LearnerHandler@475] - Sending DIFF
zoo3_1   | 2018-05-09 04:35:45,003 [myid:3] - INFO  [LearnerHandler-/172.18.0.4:38422:LearnerHandler@401] - Synchronizing with Follower sid: 2 maxCommittedLog=0x0 minCommittedLog=0x0 peerLastZxid=0x0
zoo3_1   | 2018-05-09 04:35:45,005 [myid:3] - INFO  [LearnerHandler-/172.18.0.4:38422:LearnerHandler@410] - leader and follower are in sync, zxid=0x0
zoo1_1   | 2018-05-09 04:35:45,005 [myid:1] - INFO  [QuorumPeer[myid=1]/0.0.0.0:2181:Learner@332] - Getting a diff from the leader 0x0
zoo3_1   | 2018-05-09 04:35:45,005 [myid:3] - INFO  [LearnerHandler-/172.18.0.4:38422:LearnerHandler@475] - Sending DIFF
zoo2_1   | 2018-05-09 04:35:45,007 [myid:2] - INFO  [QuorumPeer[myid=2]/0.0.0.0:2181:Learner@332] - Getting a diff from the leader 0x0
zoo3_1   | 2018-05-09 04:35:45,014 [myid:3] - INFO  [LearnerHandler-/172.18.0.3:50266:LearnerHandler@535] - Received NEWLEADER-ACK message from 1
zoo3_1   | 2018-05-09 04:35:45,015 [myid:3] - INFO  [QuorumPeer[myid=3]/0.0.0.0:2181:Leader@962] - Have quorum of supporters, sids: [ 1,3 ]; starting up and setting last processed zxid: 0x100000000
zoo3_1   | 2018-05-09 04:35:45,014 [myid:3] - INFO  [LearnerHandler-/172.18.0.4:38422:LearnerHandler@535] - Received NEWLEADER-ACK message from 2
```

When zookeeper is ready, start vault:
```
$ docker-compose up -d vault
```

Check vault log:
```
$ docker-compose logs vault
vault_1  | 2018-05-09 04:36:53.067257 I | Connected to 172.18.0.4:2181
vault_1  | 2018-05-09 04:36:53.128491 I | Authenticated: id=144124931747086336, timeout=4000
vault_1  | 2018-05-09 04:36:53.129329 I | Re-submitting `0` credentials after reconnect
vault_1  | ==> Vault server configuration:
vault_1  |
vault_1  |              Api Address: http://0.0.0.0:8200
vault_1  |                      Cgo: disabled
vault_1  |          Cluster Address: https://0.0.0.0:8201
vault_1  |               Listener 1: tcp (addr: "0.0.0.0:8200", cluster address: "0.0.0.0:8201", tls: "disabled")
vault_1  |                Log Level: info
vault_1  |                    Mlock: supported: true, enabled: true
vault_1  |                  Storage: zookeeper (HA available)
vault_1  |                  Version: Vault v0.10.1
vault_1  |              Version Sha: 756fdc4587350daf1c65b93647b2cc31a6f119cd
vault_1  |
vault_1  | ==> Vault server started! Log data will stream in below:
```

# Vault UI
Browse http://localhost:8200/ui

# Down
Down the whole cluster:
```
$ docker-compose down
```

