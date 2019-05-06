# IP Address Manager
Manage IP use and reallocation of IP addresses from a pre-allocated pool.   Useful in bridging dynamic provisioning for clouds when full SDN is not yet feasible for the org.

A service to manage a set of IP addresses so they can be allocated to virtual machines as they are needed.  Works against a pre-allocated pool of IP Addresses where they appear to most of the network as static provisioned IP addreses.   It allows the provisioning scripts to obtain a IP address in a specific IP address upon request and then continue to provisioning process using the IP address obtained.  

It differs from DNS dynamic IP mapping because most of the network infrastructure and code behaves as if the code is running against a well known static IP address.    It is more flexible because it allows IP addresses from the common pool to be released an reused.

# Assumptions
* IP addresses are supplied 
* Each security zone can be defined as a subset of IP addresses.
* IP addresses can be released and re-used by another VM.
* Once allocated a static IP address can be used indefinitely by a VM.
* We need to support managing IP addresses for multiple enviornments simutaneously without conflict.
* Will not require hardcoding IP addresses in any Scripts or YML files such as Heat scripts.

# Requirements
* Must be able to manage a set of 1 to 5 million unique IP addresses.

* Security Requirements
  * Must be able to separate IP addresses into security groups where a range of IP addresses can be defined for each security zone.
  * Must be able to manage a seprate set of addresses for different clouds.
  * Must be able to manage a separate set of addresses for different security zones.
  * Must be able to manage a separate set of addresses for different clousts + different security zones.
  * Security tools must receive a logging event when a IP address is allocated to a specific VM.
  * Secuirty tools must receive a logging event when a IP address is released for use by a specific VM.
  * Service must only be accessible from designated trusted callers.

* Must Not require hardcoding IP addresses in any source scripting.
* Once assigned a IP address must remain bound to a given VM until it is explicitly released.
* Changes must be written to persistent storage immediatley before response from service.
* Must be able to produce a DNS zones file from the known IP address allocations allocations. 


* Integration Support
  * Service must allow multiple request so a single call can all allocation of many IP addresses needed for a complex enviornment.  This should be returned in a format compatible for inclusion in a Heat script.


* Provisioning & Recovery
  * Service must reload in less than 5 minutes when populated with 5 million addresses.
  * Service must start and be available in less than 15 seconds when populated with 100,000 addresses.
  * Service should reload without data loss upon restart when primary data storage mount remains available. 
  * Service must be able to restart from secondary storage when primary service & primary strorage are lost.
     * Must restart with less than 5 seconds of data loss when primary service and primary store is lost.


# Design Nodes
* We should support calling the service to get all the IP addresses needed for an enviornment so a file can be generated and the loaded with the [get file](https://docs.openstack.org/heat/latest/template_guide/software_deployment.html) command.


# API Design
.. TODO





