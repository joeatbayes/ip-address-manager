# ip Address Manager
Manage IP use and reallocation of IP addresses from a pre-allocated pool.   Useful in bridging dynamic provisioning for clouds when full SDN is not yet feasible for the org.

A service to manage a set of IP addresses so they can be allocated to virtual machines as they are needed.  Works against a pre-allocated pool of IP Addresses where they appear to most of the network as static provisioned IP addreses.   It allows the provisioning scripts to obtain a IP address in a specific IP address upon request and then continue to provisioning process using the IP address obtained.  

It differs from DNS dynamic IP mapping because most of the network infrastructure and code behaves as if the code is running against a well known static IP address.    It is more flexible because it allows IP addresses from the common pool to be released an reused.

# Assumptions
* Each security zone can be defined as a subset of IP addresses.
* IP addresses can be released and re-used by other VM.


# Requirements
* Must be able to separate IP addresses into security groups where a range of IP addresses can be defined for each security zone.
* 
* Security tools must receive a logging event when a IP address is allocated to a specific VM.
* Secuirty tools 


