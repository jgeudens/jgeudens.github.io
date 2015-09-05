---
layout: page
title: Documentation
project: modbusscope
weight: 3
---

<!--
<p class="message">
  ModbusScope documentation page
</p>-->

# ModbusScope documentation
---
ModbusScope can be configured in 2 ways. A project file (mbs) with all the settings can be loaded. And the configuration can be edited afterwards using the GUI. At the moment is not possible to save the configuration to the mbs file.
The documentation of modbus

# GUI documentation

# Project file documentation (mbs)

The mbs project file is a file in xml format. It can be edited using any text editor like for example Notepad++. All tags are case sensitive and lower case. The root tag is `modbusscope`. All sections are children of this tag. The `datalevel` attribute specifies the datalevel of the mbs file. Datalevel 2 is compatible with ModbusScope v1.x.x branch.

The project file includes most options that can be also be set using the GUI. The documentation of the mbs file is limited to a list of the tags and possible options, because the GUI options are documented above.

 The project file consists of 3 main sections/tags.

* `modbus`: Settings of the connection and general log settings
* `scope`: Definition of registers
* `view`: Settings for the view (GUI)

## Sample mbs file
This is a complete sample ModbusScope project file

```
<modbusscope datalevel="2">
	<modbus>
		<connection>
			<ip>127.0.0.1</ip>
			<port>502</port>
			<slaveid>1</slaveid>
			<timeout>1000</timeout>
			<consecutivemax>100</consecutivemax>
		</connection>

		<log>
			<polltime>1000</polltime>
			<absolutetimes>false</absolutetimes>
			<logtofile enabled="true">
				<filename>C:\...</filename>
			</logtofile>
		</log>
	</modbus>
	
	<scope>
		<register active="true">
			<address>40001</address>
			<text>Very interesting register</text>
			<unsigned>true</unsigned>
			<multiply>1</multiply>
			<divide>2</divide>
			<color>#FF0000</color>
		</register>
		<register>
			<address>40002</address>
			<text>Very interesting register 2</text>
			<unsigned>false</unsigned>
			<shift>0</shift>
			<bitmask>0xFFFF</bitmask>
		</register>
		<register active="false">
			<address>40003</address>
			<text>Very interesting register 3</text>
			<multiply>10.0</multiply>
		</register>
	</scope>
	
	<view>
		<scale>
			<xaxis mode="sliding">
				<slidinginterval>20</slidinginterval>
			</xaxis>
			<yaxis mode="minmax">
				<min>0</min>
				<max>25</max>
			</yaxis>
		</scale>
		<legend visible="true">
			<position>left</position>
		</legend>
	</view>
</modbusscope>
```

## `modbus` tag
The modbus is used for the settings that relate to the connection and general log settings.
There are 2 child tags possible.

* `connection` tag
* `log` tag

### `connection` tag

The connection tag defines the connection setttings that are used to create the connection.

```
<connection>
	<ip>127.0.0.1</ip>
	<port>502</port>
	<slaveid>1</slaveid>
	<timeout>1000</timeout>
	<consecutivemax>100</consecutivemax>
</connection>
```	


| Tag           | Explanation                                   |
| :------------- |:-------------
| `ip` 	| ip of slave (defaults to localhost)
| `port` 	| port of slave (defaults to 502)
| `slaveid` 	| slave id to poll (defaults to 1)
| `timeout` 	| Response time-out while polling (defaults to 1000)
| `consecutivemax` 	| number of registers that is read in one modbus message (defaults to 125)

### `log` tag

```
<log>
	<polltime>1000</polltime>
	<absolutetimes>false</absolutetimes>
	<logtofile enabled="true">
		<filename>C:\...</filename>
	</logtofile>
</log>
```

| Tag           | Explanation                                   |
| :------------- |:-------------
| `polltime` | Time between modbus reads (defaults to 1000)
| `absolutetimes`  | Enable absolute timestamps (default false)

#### `logtofile` tag

| Tag           | Explanation                                   |
| :------------- |:-------------
| `enabled` (attribute) | Enable logging to file (default true)
| `filename` | Path and name of log file (defaults to log file in temp directory)

## `scope` tag
The `scope` tag hold a list of modbus registers that can be logged. Every `register` tag defines the settings of a single register.

```
<scope>
	<register active="true">
		<address>40002</address>
		<text>Very interesting register 2</text>
		<unsigned>false</unsigned>
		<shift>1</shift>
		<bitmask>0xFFFF</bitmask>
		<color>#FF0000</color>
	</register>
	
	<register>
	...
	</register>
	
</scope>
```	

| Tag           | Explanation                                   |
| :------------- |:-------------
| `active` (attribute) | enable the register (default true)
| `address` | address of register (registers start at 40001)
| `color` | Color of register graph
| `text` | Description of register (shown in legend)
| `unsigned` | Specifies unsigned value (default false)
| `shift` | Number of bits to shift the value to the left (default 0)
| `bitmask` | Bitmask to apply to value (default 0xFFFF)
| `multiply` | Multiply value (default 1)
| `divide` | Divide value (default 1)

## `view` tag
The view tag contains settings that control the representation of the data and the graph component.

The view tag can have next child tags

* scale
* legend

```
<view>
	<scale>
		<xaxis mode="sliding">
			<slidinginterval>20</slidinginterval>
		</xaxis>
		<yaxis mode="minmax">
			<min>0</min>
			<max>25</max>
		</yaxis>
	</scale>
	<legend visible="true">
		<position>left</position>
	</legend>
</view>
```

### `scale` tag

The settings in this tag control the scaling of the x- and y axis.

#### `xaxis` tag

| Tag           | Explanation                                   |
| :------------- |:-------------
| `mode` (attribute)  | Set scaling mode off x-axis (options are auto or sliding), default is auto
| `slidinginterval` | When scaling mode is activated, this tag defines the number of sliding interval

#### `yaxis` tag

| Tag           | Explanation                                   |
| :------------- |:-------------
| `mode` (attribute)  | Set scaling mode off x-axis (options are auto or minmax), default is auto
| `min` | When minmax mode is activated, this tag defines the minimum
| `max` | When minmax mode is activated, this tag defines the maximum

### `legend` tag

The settings in this tag controls the legend visibility and position

| Tag           | Explanation                                   |
| :------------- |:-------------
| `visible` (attribute)  | Show legend (default true)
| `position` | position of legend (left, middle(default) or right)
