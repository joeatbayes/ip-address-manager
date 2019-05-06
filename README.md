# ip Address Manager
Manage IP use and reallocation of IP addresses from a pre-allocated pool.   Useful in bridging dynamic provisioning for clouds when full SDN is not yet feasible for the org.

A service to manage a set of IP addresses so they can be allocated to virtual machines as they are needed.  Works against a pre-allocated pool of IP Addresses where they appear to most of the network as static provisioned IP addreses.   It allows the provisioning scripts to obtain a IP address in a specific IP address upon request and then continue to provisioning process using the IP address obtained.  

It differs from DNS dynamic IP mapping because most of the network infrastructure and code behaves as if the code is running against a well known static IP address.    It is more flexible because it allows IP addresses from the common pool to be released an reused.

# Assumptions
* Each security zone can be defined as a subset of IP addresses.
* IP addresses can be released and re-used by another VM.
* Once allocated a static IP address can be used indefinitely by a VM.
* We need to support managing IP addresses for multiple enviornments simutaneously without conflict.
* 

# Requirements
* Must be able to manage a set of 1 to 5 million unique IP addresses.
* Must be able to separate IP addresses into security groups where a range of IP addresses can be defined for each security zone.
* Security tools must receive a logging event when a IP address is allocated to a specific VM.
* Secuirty tools must receive a logging event when a IP address is released for use by a specific VM.
*
* Once assigned a IP address must remain bound to a given VM until it is explicitly released.
* Changes must be written to persistent storage immediatley before response from service.
* Service must reload wiht all known mapping quickly in less than 5 minutes.
* Service must update boht local and remote data store to allow re-setup after reboot.
* Service should reload without data loss provided primary data storage mount remains available upon provisioning a new service.
* Service on restart must be able to restart with less than 5 seconds allocation loss from secondary storage upon failure of primary service and primary storage.



