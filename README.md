# IP Address Manager
Manage IP use and reallocation of IP addresses from a pre-allocated pool.   Useful in bridging dynamic provisioning for clouds when full SDN is not yet feasible for the org.

A service to manage a set of IP addresses so they can be allocated to virtual machines as they are needed.  Works against a pre-allocated pool of IP Addresses where they appear to most of the network as static provisioned IP addreses.   It allows the provisioning scripts to obtain a IP address in a specific IP address upon request and then continue to provisioning process using the IP address obtained.  

It differs from DNS dynamic IP mapping because most of the network infrastructure and code behaves as if the code is running against a well known static IP address.    It is more flexible because it allows IP addresses from the common pool to be released an reused.

* NAT logging is an activie in design for neutron and delivery of that feature is unkown with requirements under debate.   Best case delivery is 14 months could easily be longer.


## Short solution:
*  Mapps all VM that need to listen on a socket or open a socket to another host are bound to a floating IP so they appear to the rest of network as statically bound IP.   This allows the Palo to direclty log all socket creation events to or from the VM. 
  * Need to confrim palo ability to log from the VM by confirming that all traffic to other VM provided source address as floating IP. 
* Short term: for MVP leaves all resources that need a VM with floating IP hard coded in YML files that are checking to source control and manually edited.
* Medium term:  Dynamically allocate floating iP from a set known to be available to the appliance.   And reuse those IP for same enviornment when re-provisioning.    May require about 2 to 3 weeks from time short term is working. 
* Ability to correlate the use of a given IP address at a given period of time for VM associated at that time. 
* Limitations
  * Can not allow teams like Bride to spin up as many VM as desired using sandbox because we can not allow them to create any IP that reach out other than those assigned to a floating IP. 
  * Can not gaurnatee the team would use floating IP and as such we could not allow them to run any provisioning scripts.
  * Would require creating of tooling that controlls provisioning that could be ran on request by team. 
  * Would require Devops to manually audit and approve any Heat script changes.  Only Devops or trusted Devops scripts / services could invoke Heat scripts. 
* Caveats:  CISCO may allow sandboxes in a less regulated non-prod cloud. Could make it easier to use this for longer period of time.

## Medium / Long Term Work Around.
* Alternative of a patch to get extra logging to allow direct mapping of port assignment to specific VM could take as little as 10 days but vendor has not comitted.   Currently socket create in debug logging and bring up to info logging.  We would still have to write a utility to uses this information to correlate more specific creation events to enrich logging to make it easy for logrithm to correlate with the Palo logs.   Main concern here log volumes and brownout under heavy connetion streams.  Also has a moderate risk of additional coding to cover edge cases.    Expected delivery 3 to 4 weeks.  Worst case would be 2 to 3 months. 
  * Requires Kafaka that can scale to handle load for connection requets. 


# Alternatives 
* Using DHCP do do initial allocation then to re-use specified DHCP allocated IP as parameters into the heat stack.  First time spinning up then DHCP allocate then if re-spin up then re-use and spin back up.  DHCP weakness is if a IP address was released and then re-assigned before the same enviornment was repovisioned then may not get the same IP address back or collision would occur.   Separations of different IP ranges for differnt security zones will be supported using dynamically created A records within heat. A script using a dynamically parameter passed into the heat script that is filled in and Heat will register with a subnet then the DHP 



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





