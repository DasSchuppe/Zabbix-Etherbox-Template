# Zabbix-Etherbox-Template

## Description
This Zabbix Template is for Monitoring Etherbox Device by MessPC (https://www.messpc.de/ethernetbox.php). 
It provides basic Information about the device ans specific Information for every Senonsr

Version / Zabbix version:
-------------------------

- 7.2 (and above)

## Macros used

|Name|Description|Default|Type|
|----|-----------|-------|----|
|{$WARN_HUM}|<p>-</p>|`65`|Text macro|
|{$MAX_HUM}|<p>-</p>|`75`|Text macro|
|{$WARN_TEMP}|<p>-</p>|`24`|Text macro|
|{$MAX_HUM}|<p>-</p>|`28`|Text macro|

## Template links

There are no template links in this template.

- ICMP Ping

## Discovery rules

|Name|Description|Type|Key and additional info|
|----|-----------|----|----|
|Etherbox Discovery|<p>-</p>|`SNMP agent`|etherbox.discovery<p>Update Interval: 1h</p>|

## Items collected

|Name|Description|Type|Key and additional info|
|----|-----------|----|----|
|Firmware Version|<p>Firmware Version of the Device</p>|`SNMP agent`|etherbox.firmware<p>Update Interval: 60m</p>|
|ICMP Ping: ICMP loss|<p>-</p>|`Simple Check`|icmppingloss<p>Update Interval: 1m</p>|
|ICMP Ping: ICMP ping|<p>-</p>|`Simple Check`|icmpping<p>Update Interval: 1m</p>|
|ICMP Ping: ICMP response time|<p>-</p>|`Simple Check`|icmppingsec<p>Update Interval: 1m</p>|
|IP-Address|<p>The IP Address of the Device</p>|`SNMP agent`|etherbox.ip<p>Update Interval: 1m</p>|
|Location|<p>The Location of this Device</p>|`SNMP agent`|etherbox.location<p>Update Interval: 10m</p>|

## Item prototypes

|Name|Description|Type|Key and additional info|
|----|-----------|----|----|
|Sensor {#SNMPINDEX} - Type|<p>The Type of the connected Sensor</p>|`SNMP agent`|etherbox.type.[{#SNMPINDEX}]<p>Update Interval: 10m</p>|
|Sensor {#SNMPINDEX} - Value|<p>The Value of the connected Sensor</p>|`SNMP agent`|etherbox.value.[{#SNMPINDEX}]<p>Update Interval: 1m</p>|


## Triggers

|Name|Description|Expression|Priority|
|----|-----------|----------|--------|
|ICMP Ping: ICMP Ping: Unavailable by ICMP ping|<p>-</p>|<p>**Expression**: max(/Template Etherbox SNMP V2/icmpping,#3)=0</p><p>**Recovery expression**: </p>|High|
|ICMP Ping: ICMP Ping: High ICMP ping loss|<p>-</p>|<p>**Expression**: min(/Template Etherbox SNMP V2\icmppingloss,5m)>{$ICMP_LOSS_WARN} and min(/Template Etherbox SNMP V2/icmppingloss,5m)<100</p><p>**Recovery expression**: </p>|Warning|
|ICMP Ping: ICMP Ping: High ICMP ping response time|<p>-</p>|<p>**Expression**: avg(/Template Etherbox SNMP V2/icmppingsec,5m)>{$ICMP_RESPNSE_TIME_WARN}</p><p>**Recovery expression**: </p>|Warning|

## Trigger prototypes

|Name|Description|Expression|Priority|
|----|-----------|----------|--------|
|{HOSTNAME} Contact Sensor different than 0 (Water detected)|<p>-</p>|<p>**Expression**: (last(/Template Etherbox SNMP V2/etherbox.value.[{#SNMPINDEX}])>0) and (last/Template Etherbox SNMP V2/etherbox.type.[{#SNMPINDEX}]=4)</p><p>**Recovery expression**: </p>|Disaster|
|{HOSTNAME} Humidity > {$MAX_HUM}|<p>-</p>|<p>**Expression**: (last(/Template Etherbox SNMP V2/etherbox.type.[{#SNMPINDEX}])=3) and (last/Template Etherbox SNMP V2/etherbox.value.[{#SNMPINDEX}]>{$MAX_HUM})</p><p>**Recovery expression**: </p>|High|
|{HOSTNAME} Humidity > {$WARN_HUM}|<p>-</p>|<p>**Expression**: (last(/Template Etherbox SNMP V2/etherbox.type.[{#SNMPINDEX}])=3) and (last/Template Etherbox SNMP V2/etherbox.value.[{#SNMPINDEX}]>{$WARN_HUM})</p><p>**Recovery expression**: </p>|Warn|
|{HOSTNAME} Temperature > {$MAX_TEMP}|<p>-</p>|<p>**Expression**: (last(/Template Etherbox SNMP V2/etherbox.type.[{#SNMPINDEX}])=1) and (last/Template Etherbox SNMP V2/etherbox.value.[{#SNMPINDEX}]>{$MAX_TEMP})</p><p>**Recovery expression**: </p>|High|
|{HOSTNAME} Temperature > {$WARN_TEMP}|<p>-</p>|<p>**Expression**: (last(/Template Etherbox SNMP V2/etherbox.type.[{#SNMPINDEX}])=1) and (last/Template Etherbox SNMP V2/etherbox.value.[{#SNMPINDEX}]>{$WARN_TEMP})</p><p>**Recovery expression**: </p>|Warn|
