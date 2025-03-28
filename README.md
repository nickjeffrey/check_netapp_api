# check_netapp_api
nagios check for NetApp ONTAP storage system health using HTTPS-based REST API

# Requirements
perl, jq, usernname and password credentials with API access on NetApp ONTAP storage

# Configuration

Create a username/password on the NetApp ONTAP storage system with access to the REST API.  For example:
```
   security login create -username nagios -application http,ontapi -authmethod password -role readonly
```


You will need a section in the services.cfg file on the nagios server that looks similar to the following.
```
    define service{
       #syntax is check_netapp_api!username!password
       use                             generic-service
       host_name                       netappfiler01
       service_description             Netapp health check
       check_command                   check_netapp_api!api_username!api_password
       }
```
You will also need a command definition similar to the following in commands.cfg on the nagios server
```
    # 'check_netapp_api' command definition
    define command{
       command_name    check_netapp_api
       command_line    $USER1$/check_netapp_api  -H $HOSTADDRESS$ --user $ARG1$ --password $ARG2$
       }
```

# Output
You will see output similar to the following
```
    NetApp OK - health:ok ontap_version:9.16.1 node_count:2 aggregate_count:2 volume_count:7 failed_fan_count:0 failed_power_supply_count:0
```
```
    NetApp WARN - Aggregate aggr1 is 92% full.  health:ok ontap_version:9.16.1 node_count:2 aggregate_count:2 volume_count:7 failed_fan_count:0 failed_power_supply_count:0
```
```
    NetApp WARN - Volume vol1 is 94% full.  health:ok ontap_version:9.16.1 node_count:2 aggregate_count:2 volume_count:7 failed_fan_count:0 failed_power_supply_count:0
```
