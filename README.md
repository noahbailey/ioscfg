# ioscfg

A simple desired-state config system for Cisco IOS routers and multilayer switches. 

This is a work in progress, and something I only work on occasionally or when I find a feature I feel it really needs. Feel free to use this, abuse this, fork this, and hopefully make it better. 

requires python 3.6+

## Usage

Modify the configuration files to reflect your desired state of configuration. 

Command line arguments: 

Argument | Input | Effect
--- | --- | ---
--config_path | Path |The relative/full path of the config directory. By default it will use the current directory
--switch | Switch | Uses switch config mode with layer-2 ports and VLANs
--router | Switch | Uses router config mode with routed ports and dynamic routing protocols
--multilayer | Switch | Configures both layer-2 ports and routing protocols, as well as layer-3 virtual interfaces. 

The main config databases are parsed from three comma seperated text files:

 File | Configuration
 --- | ---
 svi.csv | configure the layer-3 virtual interfaces
 vlan.csv  | configure vlan database on the device 
 switch.csv | Map ports to vlans, and set up trunks

The rest of the configuration is contained in the `config.ini`
+ hostname & domain
+ authentication
  + SSH and console
+ routing
  + eigrp routing
  + ospf routing

### EIGRP
The system uses EIGRP address families (supported in 15+). In the config file, only networks and interfaces are required. Though not recommended, it's probably okay to use `0.0.0.0` (don't tell the CCNPs that or they'll smack you :P )
### OSPF
All OSPF config is done by area, which in the `config.ini` file is expressed by using lists of networks in each area. 
````
area_51 = [
        "172.16.200.1 255.255.255.252",
        "10.123.11.0 255.255.255.0"
 ] 
````
The parser will assume this format, so even slight deviations may cause runtime errors. I'm trying to find a smarter way to do this, but bad design decisions come naturally to me... 

### Todo: 

- ACL configuration
- spanning-tree mode & role
- OSPFv3 config
- BGP and MP-BGP
