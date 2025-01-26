Usefull site: https://ccnasec.com
Some intresting labs:
* [Setup and manage passwords](https://ccnasec.com/2-6-1-2-lab-securing-the-router-for-administrative-access-instructor-version/)
* [AAA and Radius](https://ccnasec.com/3-6-1-1-lab-securing-administrative-access-using-aaa-and-radius-instructor-version/)
* [Zone based policy](https://ccnasec.com/4-4-1-2-lab-configuring-zone-based-policy-firewalls-instructor-version/)
* [Securing layer 2](https://ccnasec.com/6-3-1-1-lab-securing-layer-2-switches-instructor-version/)
* [Site-To-Site VPN](https://ccnasec.com/8-4-1-3-lab-configuring-a-site-to-site-vpn-using-cisco-ios-instructor-version/)

## Configurations

* vlan
    * Creating a vlan
        ```
        Switch(config)# vlan <number>
        Switch(config-vlan)# exit
        ```
    * Port assignment
        * RoaS (Router on a Stick)
            Allows for communication between vlans which have had an interface(subinterface) made for them
            ```
            Router(config)# interface <interface.subinterface>
            Router(config-if)# encapsulation dot1q <vlan>
            Router(config-if)# ip address <address> <mask>
            Router(config)# interface <interface>
            Router(config-if)# no shutdown
            ```
            Usually in <interface.subinterface> ".subinterface" you put vlan number for convenience
        * Access mode
            ``` Switch
            Switch(config)# int <interface type> <port>
            Switch(config-if)# sw mod access
            Switch(config-if)# sw access <vlan number>
            ```
        * Trunk mode
            ``` Switch
            Switch(config)# int <interface type> <port>
            Switch(config-if)# sw mode trunk
            ```
            * Native vlan
                ``` Switch
                Switch(config)# int <interface type> <port>
                Switch(config-if)# sw mode trunk
                Switch(config-if)# sw trunk native vlan <vlan number>
                ```
    * Name vlan
        ``` Switch
        Switch(config-vlan)# name <name>
        ```

* vtp
    ``` Switch
    Switch(config)# vtp mode <vtp mode>
    Switch(config)# vtp domain <domain name>
    Switch(config)# vtp password <password name>
    ```

    * Possible vtp modes:
        * transparent
        * client
        * server
        * off
    * vtp pruning
    ``` Switch
    Switch(config)# vtp pruning
    ```

* stp
    * Root node
        ``` Switch
        Switch(config)# spannig-tree vlan <vlan> priority <numer>
        ```
        * Setting primary and secondary Root
        ``` Switch
        Switch(config)# spannig-tree vlan <vlan> root <primary|secondary>
        ```
    * Port priority
        ``` Switch
        Switch(config-if)# spannig-tree port-priority <priority>
        ```
        ``` Switch
        Switch(config-if)# spannig-tree vlan <vlan> port-priority <priority>
        ```
    * Path cost
        ``` Switch
        Switch(config-if)# spannig-tree cost <cost>
        ```
        ``` Switch
        Switch(config-if)# spannig-tree vlan <vlan> cost <cost>
        ```
    * Timers
        ``` Switch
        Switch(config)# spannig-tree vlan <vlan> hello-time <seconds>
        ```
        ``` Switch
        Switch(config)# spannig-tree vlan <vlan> forward-time <seconds>
        ```
        ``` Switch
        Switch(config)# spannig-tree vlan <vlan> max-age <seconds>
        ```
    * PortFast
        ``` Switch
        Switch(config-if)# spannig-tree portfast
        ```
    * BPDU Guard
        ``` Switch
        Switch(config-if)# spannig-tree bpduguard enable
        ```
    * BPDU filtering
        ``` Switch
        Switch(config-if)# spannig-tree bpdufilter enable
        ```
    * UplinkFast
         ``` Switch
        Switch(config-if)# spannig-tree uplinkfast
        ```
    * BackboneFast
         ``` Switch
        Switch(config-if)# spannig-tree backbonefast
        ```
    * Root Guard
         ``` Switch
        Switch(config-if)# spannig-tree guard root
        ```
    * Nonegotiate
        ```
        Switch(config)# interface <interface>
        Switch(config-if)# switchport nonegotiate
        ```
    * Loopguard
        ```
        Switch(config)# spanning-tree loopguard default
        ```

* mstp
    ``` Switch
    Switch(config)# spanning-tree mst configuration
    Switch(config-mst)# instance <instance> vlan <vlan>
    Switch(config-mst)# name <name>
    Switch(config-mst)# revision <revision>
    Switch(config)# spanning-tree mode mst
    ```
    To jest tak jakby commit dla `spanning-tree mst configuration`
    ```
    Switch(config)# spanning-tree mode mst
    ```
    * Root switch
        ``` Switch
        Switch(config)# spanning-tree mst <instance> root <primary|secondary>
        ```
    * priority
        ``` Switch
        Switch(config)# spanning-tree mst <instance> priority <priority>
        ```
    * path cost
        ``` Switch
        Switch(config-if)# spanning-tree mst <instance> cost <cost>
        ```
    * Timers
        ``` Switch
        Switch(config)# spanning-tree mst hello-time <seconds>
        Switch(config)# spanning-tree mst forward-time <seconds>
        Switch(config)# spanning-tree mst max-age <seconds>
        ```
    * Max hop count
        ``` Switch
        Switch(config-if)# spanning-tree mst max-hops <hop-count>
        ```
    * Link type
        ``` Switch
        Switch(config-if)# spanning-tree link-type <point-to-point/shared>
        ```

* Switch Remote Access
    * Ensure you have remote access to switch
        ``` Switch
        Switch(config)# int vlan <vlan>
        Switch(config-if)# ip add <ip_address> <mask>
        #OPTIONAL Switch(config)# ip default-gateway <ip_address_of_your_default_gateway>
        ```
    * ssh
        * Setup
            * Domain setup
                ```
                Router(config)# ip domain-name <domain-name>
                Router(config)# username <username> privilege 15 algorithm-type scrypt secret <password>
                ```
            * Line configuration
                ```
                Router(config)# line vty <number> <number> (0 4) - 5 sessions
                Router(config-line)# privilege level 15
                Router(config-line)# login local
                Router(config-line)# transport input ssh
                ```
            * Keys generation
                ```
                Router(config)# crypto key generate rsa general-keys modulus 1024
                ```
            * Setting up ssh version
                ```
                Router(config)# ip ssh version 2
                ```
            * Optional setup
                ```
                Router(config)# ip ssh time-out 90
                Router(config)# ip ssh authentication-retries 2
                ```
        * Switch
            ```
            Switch(config)# enable <password|secret> <your_password>
            Switch(config)# ip domain-name <name>
            Switch(config)# crypto key generate rsa
            Switch(config)# ip ssh version 2
            Switch(config)# line vty <console_line_range>
            Switch(config-line)# transport input ssh
            Switch(config-line)# password <your_password>
            Switch(config-line)# login
            ```
    * telnet
        ``` Switch
        Switch(config)# enable <password|secret> <your_password>
        Switch(config)# line vty <console_line_range>
        Switch(config-line)# password <your_password>
        Switch(config-line)# login

        Switch(config-line)# transport input telnet
        ```
    * scp (Secure Copy)
        * Setup
            * Enabling aaa and setting it for default authorization and authentication
                ```
                Router(config)# aaa new-model
                Router(config)# aaa authentication login default local
                Router(config)# aaa authorization exec default local
                ```
            * Enabling scp
                ```
                Router(config)# ip scp server enable
                ```
        * Usage
            ```
            Router# copy scp: flash:
            ```
    * Http server
        ```
        Switch(config)# ip http server
        ```
        * https
            ```
            Switch(config)# ip http secure-server
            ```
        * Disable hhtp server (enabled by default)
            ```
            Switch(config)# no ip http server
            Switch(config)# no ip http secure-server
            ```

* ACL (Access Lists)
    * Standard ACL
        ``` Switch
        Switch(config)# access-list <acl_number_between_1_and_99> <deny|permit> <ip_source> <wildcard>
        ```
    * Extended ACL
        ``` Switch
        Switch(config)# access-list <acl_number_above_99> <deny|permit> <protocol> <ip_source> <wildcard> <ip_destination> <options>
        ``` 
        You have following options for TCP and UDP:
        * `eq <port>`
        * `lt <port>`
        * `gt <port>`
        * `neg <port>`
        * `range <port> <port>`
        * `established`
            This option can be applayed even if we specified ports in command

        Options for ICMP
        * `echo`
        * `echo-reply`
    * Connect ACL to interface
        ``` Switch
        Switch(config-if)# ip access-group <acl_number> <in|out>
        ```
    * Vlan ACL
        First define ACL
        ```
        Switch(config)# access-list 100 permit ip any host 172.18.1.3
        ```

        Create VLAN ACL and map ACL
        ```
        Switch(config)# vlan access-map NOT-TO-SERVER 10
        Switch(config-access-map)# match ip address 100
        Switch(config-access-map)# action drop
        ```

        Map rule to vlan
        ```
        vlan filter NOT-TO-SERVER vlan-list 100
        ```
    * Named ACL
        * stadard ACL
            ```
            Router(config)# ip access-list standard <acl_name>
            Router(config-std-nacl)# deny 172.18.0.0 0.0.255.255
            Router(config-std-nacl)# permit any
            ```
        * extended ACL
            ```
            Router(config)# ip access-list extended <acl_name>
            Router(config-ext-nacl)# permit tcp 172.18.0.0 0.0.255.255 host 172.16.10.10 eq 80
            Router(config-ext-nacl)# deny ip 172.18.0.0 0.0.255.255 172.16.0.0 0.0.255.255
            Router(config-ext-nacl)# permit ip any any 
            Router(config)# access-list 102 deny any any eq 80 time-range BLOCKHTTP
            Router(config)# access-list 102 permit ip any any
            ```
    * Time based ACL
        ```
        Router(config)# time-range BLOCKHTTP
        Router(config)# time-range BLOCKHTTP
        Router(config-time-range)# absolute start 08:00 23 May 2006 end 20:00 26 May 2006
        Router(config)# time-range BLOCKHTTP
        Router(config-time-range)# periodic weekdays 18:00 to 23:00
        ```
    * Examples
        * PC2 is unable to communicate with computer PC3 but should be able to communicate with Router RB
            ```
            SW1(config)#access-list 100 permit ip host 172.18.1.2 host 172.18.1.3
            
            SW1(config)#vlan access-map NOT-TO-PC 10
            SW1(config-access-map)#match ip address 100
            SW1(config-access-map)#action drop
            SW1(config-access-map)#vlan access-map NOT-TO-PC 20
            SW1(config-access-map)#action forward
            
            SW1(config)#vlan filter NOT-TO-PC vlan-list 100
            ```

        * No ping from 172.18.1.0/24 to 172.16.1.1
            ```
            RB(config)#access-list 100 deny icmp 172.18.1.0 0.0.0.255 host 172.16.1.1 echo
            RB(config)#access-list 100 permit ip any any
            
            RB(config)#int gig0/1
            RB(config-inf)#acc 100 in
            ```

        * No telnet to router RA from 172.18.1.0/24
            ```
            RB(config)#access-list 10 deny 172.18.1.0 0.0.0.255
            RB(config)#access-list 10 permit any
            
            RB(config)#line vty 0 4
            RB(config-line)#access-class 10 in
            ```

        * Access to HTTP Server (port: 80) only from PC2
            ```
            RB(config)#access-list 101 permit tcp host 172.18.1.2 host 172.16.1.1 eq 80
            RB(config)#access-list 101 deny tcp any host 172.16.1.1 eq 80
            RB(config)#access-list 101 permit ip any any
            
            RB(config)#int gig0/1
            RB(config)#acc 101 in
            ```

* storm-control
    * Configure
        ```
        Switch(config-if)# storm-control <traffic_type(broadcast/multicast/unicast)> <A_level> <B_level>
        Switch(config-if)# storm-control action <action_type(shutdown/trap)>
        ```
        A_level - If goes above enganges the "action"
        B_level - If goes below it disenganges the "action"
        Each can be given in (bps-bits/pps-packets/%)
    * Check/show
        ```
        Switch# show storm-control <interface> 
        ```

* protected port
    * Configure
        ```
        Switch(config-if)# switchport protected
        ```
        In protected mode interface cannot communicate with other interfaces in this mode
    * Check/show
        ```
        Switch# show interfaces <interface> switchport
        ```

* port blocking
    * Configure
        ```
        Switch(config-if)# switchport block <traffic_type(multicast/anycast)>
        ```
    * Check/show
        ```
        Switch# show interfaces <interface> switchport
        ```

* port-security
    * Configure
        ```
        Switch(config-if)# switchport mode access
        Switch(config-if)# switchport port-security
        Switch(config-if)# switchport port-security maximum <value> [vlan <vlan/vlan_list>]
        Switch(config-if)# switchport port-security violation <(protect/restrict/shutdown)>
        Switch(config-if)# switchport port-security mac-address <mac_address> [vlan <vlan>]
        Switch(config-if)# switchport port-security mac-address sticky
        ```
    * Configure aging
        ```
        Switch(config-if)# switchport port-security aging time <time>
        ```
    * Configure port recovery
        ```
        Switch(config)# errdisable recovery cause psecure-violation
        Switch(config)# errdisable recovery interval <time>
        ```
    * Check/show
        ```
        Switch# show port-security
        Switch# show port-security <address>
        Switch# show port-security interface <interface>
        ```

* EtherChannel
    * Configure
        ```
        Switch(config-if)# switchport mode trunk
        Switch(config-if)# channel-group <channel_group> mode <(on/auto/desirable/passive/active)>
        ```
    * Configure Load balancing
        ```
        Switch(config)# port-channel load-balance <(dst-ip/dst-mac/src-dst-ip/src-dst-mac/src-ip/src-mac)>
        ```
    * Check/show
        ```
        Switch# show etherchannel
        ```
    * Enable EtherChannel Guard
        ```
        Switch(config)# spanning-tree etherchannel guard misconfig
        ```
    * Show disabled switch ports due to misconfiguration
        ```
        Switch# show interfaces status err-disabled
        ```

* SPAN (Switched Port Analyzer)
    * Delete all existing
        ```
        Switch(config)# no monitor session all
        ```
    * Local

        * Configure
            ```
            Switch(config)# monitor session <session> source interface <interface> <(both/rx/tx)>
            Switch(config)# monitor session <session> destination interface <interface>
            ```
            "session <source> source" should have "session <source> destination" with the same <session> in order to work
    * Remote
        * Configure
            * Switch A (Source Switch)
                ```
                Switch(config)# monitor session <session> source interface <interface> <(both/rx/tx)>
                Switch(config)# vlan <vlan>
                Switch(config-vlan)# remote-span
                Switch(config-vlan)# exit
                Switch(config)# monitor session <session> destination remote vlan <vlan>
                ```
            * Switch B (Destination Switch)
                ```
                Switch(config)# vlan <vlan>
                Switch(config-vlan)# remote-span
                Switch(config-vlan)# exit
                Switch(config)# monitor session <session> source remote vlan <vlan>
                Switch(config)# monitor session <session> destination interface <interface>
                ```
        <vlan> should be the same while <session> doesn't have to be on both switches
    * Check/show
        ```
        Switch# show monitor
        Switch# show monitor session <session>
        ```

* Serial interfaces
    1. On Serial Interfaces you can additionally to normal configuration set "clockrate".
    2. Only 1 out of the 2 ports in a connection needs this set (DCE)
    3. It's syntax allows from 300 to 8'000'000 but will adjust it to closest supported standardised amount but on older devices might not work automatically

    ```
    Switch(config-if)# clockrate <value>
    ```

* Routing IPv6 RIPng
    ```
    # Global routing startup (instead of MYPROCESS you can give your name)
    Router(config)# ipv6 router rip MYPROCESS
    
    # Start routing on participating interfaces
    Router(config)# interface e0
    Router(config-if)# ipv6 rip MYPROCESS enable
    
    # optional setting - change udp port from 521 to 555 and multicast group from FF02::9 to FF02::1111
    Router(config)# ipv6 rip MYPROCESS port 555 multicast-group FF02::1111
    ```

* Configuring EIGRP routing with authentication
    - It is worth remembering that the `key chain` must be the same only on interfaces directly connected to each other
    ```
	# The key creation HAS TO BE THE SAME ON EACH DEVICE
	RouterA(config)# key chain MYCHAIN
	RouterA(config-keychain)# key 1
	RouterA(config-keychain-key)# key-string MYPASSWORD
	
	# Assigning a key to an interface
	RouterA(config)# interface s0
	RouterA(config-if)# ip authentication key-chain eigrp 10 MYCHAIN
	
	# To determine what encryption to use there's not much choice, eigrp only supports md5
	RouterA(config)# interface s0
	RouterA(config-if)# ip authentication mode eigrp 10 md5
	```

* Configuring RIPv2 routing with authentication
    ```
	# As in eigrp, we set the key
	RouterA(config)# key chain MYCHAIN
	RouterA(config-keychain)# key 1
	RouterA(config-keychain-key)# key-string MYPASSWORD
	
	# Assigning a key to an interface
	RouterA(config)# interface s0
	RouterA(config-if)# ip rip authentication key-chain MYCHAIN

	# Determine if we want to send the key in plaintext
	RouterA(config)# interface s0
	RouterA(config-if)# ip rip authentication mode md5
	```

* Static Routing
    * In static routing you add networks which the router doesn't "see" (isn't part of/isn't directly connected to)
        * Configure
            ```
            Router(config)# ip route <destination_network_ip_address> <destination_network_mask> <next_hop>
            ```
            <next_hop> - This can be either:
            a) the interface name of the router this command is being executed on which will be used to get to the <destination_ip> network
            b) ip address of the neighbour's interface which will be used to get to the <destination_ip> network
            The choice which should be used depends on which connection type is between routers (serial - better use own exit interface name, Ethernet - better to use neighbour's interface address)
        * Configure default route
            ```
            Router(config)# ip route 0.0.0.0 0.0.0.0 <next_hop>
            ```
        * Check/show
            ```
            Router# show ip route
            ```

* Dynamic Routing
    - In dynamic routing you add networks which the router "sees" (is part of/is directly connected to) and you want it to "tell" other routers about
    * RIP (v2)
        * Configure
            ```
            Router(config)# router rip
            Router(config-router)# version 2
            Router(config-router)# network <network_ip_address>
            Router(config-router)# neighbor <ip_address>
            ```
            network in routing has a 3 roles:
            * gets actualisations about specified network
            * sends actualisations about directly connected networks in specified mask
            * inform about changes in those connections

        * Configure default route redistribution
            ```
            Router(config-router)# redistribute static
            ```
            (techincally you redistribute all static routes)
            or
            ```
            Router(config-router)# default-information originate
            ```

            `redistribute static` mean you sending actualisations about all your static routes
            while `default-information originate` is more specyfic than that and limit information to only about your default routes (those with 0.0.0.0 and mask 0.0.0.0) 
        * Disable automatic route summarization
            ```
            Router(config-router)# no auto-summary
            ```
        * Check/show
            ```
            Router# show ip route
            ```
        * Timers
            ```
            Router(config-router)# timers basic <update_timer> <invalid_timer> <hold-down timer> <flush timer> 
            ```
            * Example:
                ```
                Router(config)# router rip
                Router(config-router)# timers basic 20 120 120 160
                ```
        * Passive interface
            ```
            passive-interface <interface>
            ```
            * example
                ```
                Router(config)# router rip
                Router(config-router)# network 10.4.0.0
                Router(config-router)# network 10.2.0.0
                Router(config-router)# passive-interface s0
                ```

            
    * OSPF
        ```
         Router(config)# router ospf <process_id>
         Router(config-router)# network <network_address> <wildcard> area <area_number>
         Router(config-router)# passive-interface g0/1
        ```

        Example

        ```
        Router(config)# router ospf 1
        Router(config-router)# network 192.168.1.0 0.0.0.255 area 0
        Router(config-router)# network 10.1.1.0 0.0.0.3 area 0
        Router(config-router)# passive-interface <interface>
        ```

        Check routing configuration

        ```
        Router# show ip ospf neighbor
        Router# show ip route
        ```

    * EIGRP
        ```
        Router(config)# router eigrp <autonomus system>
        Router(config-router)# network <network_address>
        ```
        * Example
            ```
            Router(config)# router eigrp 1
            Router(config-router)# network 172.16.0.0
            Router(config-router)# network 10.0.0.0 
            ```


* DHCP
    - Order in which u do exclusion, binding and common pools matter (first exclusion/binding then common pools)
    * Binding
        ```
        Router(config)# ip dhcp pool <name>
        Router(dhcp-config)# host <ip_address> <mask>
        Router(dhcp-config)# client_id <MAC_address>
        Router(dhcp-config)# default_router <ip_address>
        Router(dhcp-config)# dns-server <ip_address>
        ```
    * Exclude addresses
        ```
        Router(config)# ip dhcp excluded-address <ip_address_range_start> <ip_address_range_end>
        ```
    * Configure DHCP pool
        ```
        Router(config)# ip dhcp pool <name>
        Router(dhcp-config)# network <network_ip_address> <mask>
        Router(dhcp-config)# default-router <ip_address>
        Router(dhcp-config)# dns-server <ip_address>
        Router(dhcp-config)# netbios-name-server <ip_address>
        Router(dhcp-config)# domain-name <name>
        Router(dhcp-config)# lease <time>
        ```
    * DHCP class
        ```
        Router(dhcp-config)# class <name>
        Router(dhcp-config-class)# address range <ip_address_range_start> <ip_address_range_end>
        Router(dhcp-config-class)# exit
        ```
    * Enable DHCP
        Enabled by default
        ```
        Router(config)# service dhcp
        ```
    * Remote DHCP
        - Set this on the router which is the gateway of a network which will take addresses from DHCP on the gateway interface

        ```
        Router(config-if)# ip helper-address <ip address>
        ```

        - <ip address> address of the DHCP router's interface which router - on which the command is executed - communicates through
    * Check/show
        ```
            Router# show ip interfaces
            Router# show ip interfaces brief
            Router# show interface <interface>
            Router# show ip protocols
            Router# show ip dhcp binding
        ```
    * Snooping on a Switch
        ```
        Switch(config)# ip dhcp snooping
        Switch(config)# ip dhcp snooping vlan <vlan_list>
        Switch(config-if)# ip dhcp snooping limit rate <number>
        Switch(config)# ip dhcp snooping verify mac-address
        Switch(config)# interface <interface>
        Switch(config-if)# ip dhcp snooping trust
        Switch# show running-config dhcp
        Switch# show ip dhcp snooping
        ```
    * ip helper
        ```
        Router(config-if)# ip helper-address <ip address> 
        ```
        - Customize what is forwarding
            ```
            Router(config)# ip forward-protocol udp 107
            Router(config)# no ip forward-protocol udp 69
            ```
            Following UDP ports are forwarded by default:
            * TFTP (port 69)
            * DNS (port 53)
            * Time (port 37)
            * NetBIOS (ports 137-138)
            * ND (Network Disks – used by Sun workstations)
            * TACACS (port 49)
            * BOOTP/DHCP (ports 67-68) 

* Overall config/ Passwords
    - Can be done both on a switch and a router
    * Show current configuration
        ```
        # show running-config
        ```
    * Device name
        ```
        (config)# hostname <name>
        ```
    * Password for privilege mode
        ```
        (config)# enable secret <password>
        ```
    * Router Interface
        ```
        Router(config)# interface <interface>
        Router(config-if)# ip address <address> <mask>
        Router(config-if)# no shutdown
        ```
    * Do not store password in plan text
        ```
        Router(config)# service password-enryption
        ```
    * Set minimum length of password
        ```
        Router(config)# security passwords min-length <number>
        ```
    * Set password when typing enable
        ```
        Router(config)# enable algorithm-type scrypt secret <password>
        ```
    * Password to login to CLI on console connection
        ```
        Router(config)# line console 0
        Router(config-line)# password <password>
        Router(config-line)# exec-timeout <minutes> <seconds>
        Router(config-line)# login
        Router(config-line)# logging synchronous
        ```
        * Require password and username with specific username configuration
            ```
            Router(config-line)# login local
            ```
    * Set password for AUX port
        ```
        Router(config)# line aux 0
        Router(config-line)# password <password>
        Router(config-line)# exec-timeout <minutes> <seconds>
        Router(config-line)# login
        ```
        * Require password and username with specific username configuration
            ```
            Router(config-line)# login local
            ```
    * Set password for telnet
        ```
        Router(config)# line vty <number> <number> (0 4) - 5 sessions
        Router(config-line)# password <password>
        Router(config-line)# exec-timeout <minutes> <seconds>
        Router(config-line)# transport input telnet
        Router(config-line)# login
        ```
        * Require password and username with specific username configuration
            ```
            Router(config-line)# login local
            ````
    * Setting banner for login screen
        ```
        Router(config)# banner motd $DEFINITLY NON ANTISEMITIC BANNER$
        ```
    * Create new user
        ```
        Router(config)# username <username> algorithm-type scrypt secret <password>
        ```

* logging/remote logging
    ```
    Router(config)# logging on
    Router(config)# logging host <ip_dest>
    Router(config)# service timestamps log datetime year
    Router(config)# service sequence-numbers
    ```

    On default levels, logs will be send to logging host

    ```
    Router(config)# logging console <level>
    Router(config)# logging monitor <level>
    Router(config)# logging trap <level>
    ```

* NAT/PAT (Network Address Translation/Port Address Translation)
    * Specify inside and outside ports
        ```
        Router(config-if)# ip nat inside
        ```
        ```
        Router(config-if)# ip nat outside
        ```
    * Create nat pool
        ```
        Router(config)# ip nat pool <pool_name> <ip_address_range_start> <ip_address_range_end> netmask <mask>
        ```
    * Static NAT
        * Example
        ```
        # Defining where the private interfaces are and where are public
        Router(config)# int e0/0
        Router(config-if)# ip nat inside
        Router(config)# int s0/0
        Router(config-if)# ip nat outside

        # Mapping a private address to a public address
        Router(config)# ip nat inside source static 172.16.1.1 158.80.1.40
        ```
    * DNAT (Dynamic NAT)
        ```
        Router(config)# access-list <number> permit <ip_addres> <wildcard>
        Router(config)# ip nat inside source list <number> pool <pool_name>
        ```
        * Example
            ```
            Router(config)# int e0/0
            Router(config-if)# ip nat inside
            Router(config)# int s0/0
            Router(config-if)# ip nat outside
            
            # Setting up a pool of public addresses
            Router(config)# ip nat pool POOLNAME 158.80.1.1 158.80.1.50 netmask 255.255.255.0
            
            # Setting up a pool of private addresses
            Router(config)# ip nat inside source list 10 pool POOLNAME
            Router(config)# access-list 10 permit 172.16.1.0 0.0.0.255	
            ```
    * PAT
        ```
        Router(config)# access-list <number> permit <ip_addres> <wildcard>
        Router(config)# ip nat inside source list <number> <interface <port>|pool <pool_name>> overload
        ```
        * Example
            ```
            Router(config)# int e0/0
            Router(config-if)# ip nat inside
            Router(config)# int s0/0
            Router(config-if)# ip nat outside
            Router(config)# ip nat inside source list 10 interface Serial0/0 overload
            Router(config)# access-list 10 permit 172.16.1.0 0.0.0.255
            ```

* DNS Lookup
    * Don't translate commands to DNS requests
        ```
        Router(config)# no ip domain-lookup
        ```

* Roles Configuration
    - AAA is required for views to work properly
    * Enabling view for root
        ```
        Router# enable view
        ```
    * Adding new views
        ```
        Router(config)# parser view admin1
        Router(config-view)# secret admin1pass
        Router(config-view)# commands ? ...
        ```
    * Enable specific view
        ```
        Router# enable view <view-name>
        ```
    * Example commands
        ```
        Router(config-view)# commands exec include all show
        Router(config-view)# commands exec include all config terminal
        Router(config-view)# commands exec include all debug

        Router(config-view)# commands exec include show version
        Router(config-view)# commands exec include show interfaces
        Router(config-view)# commands exec include show ip interface brief
        Router(config-view)# commands exec include show parser view
        ```

* Image Resistance
    * Securing Cisco Image and archived configs
        ```
        Router(config)# secure boot-image
        Router(config)# secure boot-config
        ```
    * Validate
        ```
        Router# show secure bootset
        Router# show flash:
        ```

* Simple Network Management Protocol (SNMPv3) with ACL
    * Creating ACL
        ```
        Router(config)# ip access-list standard PERMIT-SNMP
        Router(config-std-nacl)# permit <network> <mask>
        ```
    * Configuring view, group and username
        ```
        Router(config)# snmp-server view SNMP-RO iso included
        Router(config)# snmp-server group SNMP-G1 v3 priv read SNMP-RO access PERMIT-SNMP
        Router(config)# snmp-server user SNMP-Admin SNMP-G1 v3 auth sha Authpass priv aes 128 Encrypass
        ```
    * Validation
        ```
        Router# show snmp group
        Router# show snmp user
        ```

* Syslog configuration
    * Setup
        * Initializing NTP
            ```
            Router(config)# service timestamps log datetime msec
            Router(config)# logging host <ip-address>
            ```
        * Setting moment of log trap
            |number|level|description|
            |0|emergencies|System is unusable|
            |1|alerts|Immediate action required|
            |2|critical|Critical conditions|
            |3|errors|Error conditions|
            |4|warnings|Warning conditions|
            |5|notifications|Normal but significant condition|
            |6|informational|Informational messages|
            |7|debugging|Debugging messages|
            ```
	        Router(config)# logging trap <0-7>
            ```
        * Show logging status
            ```
            Router# show logging
            ```

* Local authorization
    * Creating new user
        ```
        Router(config)# username <username> algorithm-type scrypt secret <password>
        ```
    * Setup for CLI, telnet, aux
        * CLI
            ```
            Router(config)# line console 0
            Router(config-line)# login local
            ```
        * Telnet
            ```
            Router(config)# line vty <number> <number> (0 4) - 5 sessions
            Router(config-line)# login local
            Router(config-line)# transport input telnet
            ```
        * Aux
            ```
            Router(config)# line aux 0
            Router(config-line)# login local
            ```

* Configuring local authorization with AAA
    * Creating local user with highest privilege
        ```
        Router(config)# username <username> privilege 15 algorithm-type scrypt secret <password>
        ```
    * Enabling AAA service and setting as default login method
        ```
        Router(config)# aaa new-model
	    Router(config)# aaa authentication login default local-case none
        ```
    * Requiring AAA for SSH / telnet authentication
        ```
        Router(config)# aaa authentication login TELNET_LINES local
        Router(config)# line vty <number> <number> (0 4) - 5 sessions
        Router(config-line)# login authentication TELNET_LINES
        ```
    * Enabling AAA debugging
        ```
        Router# debug aaa authentication
        ```

* Configuring a centralized authentication system using RADIUS
    - First, we need to configure WinRadius, add some user there, etc. We can then test our configuration using RadiusTest included with WinRadius
    * Router Configuration
        ```
        Router(config)# aaa new-model
        Router(config)# aaa authentication login default group radius none
        ```
    * This command is used to exchange passwords between radius and router for mutual authentication
        ```
        Router(config-radius-server)# key WinRadius
        ```
    * Configure login with radius when authenticating via telnet / ssh
        ```
        Router(config)# aaa authentication login TELNET_LINES group radius
        Router(config)# line vty <number> <number> (0 4) - 5 sessions
        Router(config-line)# login authentication TELNET_LINES
        ```
    * Example of setting ip address
        ```
        Router(config)# radius server CCNAS
        Router(config-radius-server)# address ipv4 192.168.1.3
        ```

* Configuration Zone-Based Firewall
    * Establishment of zones. Each zone will have a different security policy, etc.
        ```
        Router(config)# zone security INSIDE
        Router(config)# zone security CONFROOM
        Router(config)# zone security INTERNET
        ```
    * Creation of security policies
        - creating class-map, which allow traffic from inside to internet match-any -> At least one must match
        ```
        Router(config)# class-map type inspect match-any INSIDE_PROTOCOLS
        Router(config-cmap)# match protocol tcp
        Router(config-cmap)# match protocol udp
        Router(config-cmap)# match protocol icmp
        ```
        * Example of protocols with higher layers
            ```
            Router(config)# class-map type inspect match-any CONFROOM_PROTOCOLS 
            Router(config-cmap)# match protocol http
            Router(config-cmap)# match protocol https
            Router(config-cmap)# match protocol dns
            ```
    * After creating `class-maps`, we can create `policy-maps`. In this we assign the appropriate classes to the policy and tell what to do when the packages are zmatchowane
        ```
        Router(config)# policy-map type inspect INSIDE_TO_INTERNET
        Router(config-pmap)# class type inspect INSIDE_PROTOCOLS
        Router(config-pmap-c)# inspect
        ```
    * Creating zone pairs. Zone pairs give us an indication of the direction in which the security policy should operate.
        ```
        Router(config)# zone-pair security INSIDE_TO_INTERNET source INSIDE destination INTERNET
        ```
        * Validation
            ```
            Router# show zone-pair security
            ```
    * After creating all the pairs, policies, maps and other crap, we can apply the policies.
        ```
        Router(config)# zone-pair security INSIDE_TO_INTERNET
        Router(config-sec-zone-pair)# service-policy type inspect INSIDE_TO_INTERNET
        ```
        * More detailed policy information
            ```
            Router# show policy-map type inspect zone-pair
            ```
    * After applying the policies, we can assign the appropriate interfaces to the zone
        ```
        Router(config)# interface <interface>
        Router(config-if)# zone-member security INSIDE
        Router(config)# interface <interface>
        Router(config-if)# zone-member security INTERNET
        ```
        * Validate
            ```
            Router# show zone security
            ```

* IPSec Site-to-Site VPN
    * Enabling IKE policy on the router
        ```
        Router(config)# crypto isakmp enable
        Router(config)# crypto isakmp policy <priority> - lower is higher
        ```
    * Example policy configuration
        ```
        Router(config)# crypto isakmp policy <priority> - lower is higher
        Router(config-isakmp)# hash sha
        Router(config-isakmp)# authentication pre-share
        Router(config-isakmp)# group 14
        Router(config-isakmp)# lifetime 3600
        Router(config-isakmp)# encryption aes 256
        Router(config-isakmp)# end
        ```
    * Validation
        ```
        Router# show crypto isakmp policy
        ```
    * Configuring Keys
        - IP indicates the router to which we will be tunneling
        ```
        Router(config)# crypto isakmp key <password> address <ip-address>
        Router(config)# crypto ipsec transform-set 50 esp-aes 256 esp-sha-hmac
        Router(config)# crypto ipsec security-association lifetime seconds <seconds>
        ```
    * Defining the traffic of interest and assigning it to the map
        * Example ACL of traffic
            ```
            Router(config)# access-list <access-list-id> permit ip 192.168.1.0 0.0.0.255 192.168.3.0 0.0.0.255
            ```
        * Setting map for ACL
            ```
            Router(config)# crypto map CMAP 10 ipsec-isakmp
            Router(config-crypto-map)# match address <access-list-id>
            ```
        * Setting addresses, etc
            ```
            Router(config-crypto-map)# set peer <ip-address>
            Router(config-crypto-map)# set pfs <group>
            ```
        * Tag below must agree with `Router(config)# crypto ipsec transform-set <ID> esp-aes 256 esp-sha-hmac` in Configuring Keys
            ```
            Router(config-crypto-map)# set transform-set <ID>
            Router(config-crypto-map)# set security-association lifetime seconds <seconds>
            Router(config-crypto-map)# exit
            ```
        * Applying map
            ```
            Router(config)# interface S0/0/0
	        Router(config-if)# crypto map CMAP
            ```

* Configuring a point-to-point tunnel - GRE VPN
    * Configuring the interface for the GRE tunnel
        ```
        WEST(config)# interface tunnel 0
        WEST(config-if)# ip address 172.16.12.1 255.255.255.252

        # Source is where the package comes from

        WEST(config-if)# tunnel source s0/0/0

        # Dest is where we send the package

        WEST(config-if)# tunnel destination 10.2.2.1 # <ip EAST>
        
        # Another example
        
        EAST(config)# interface tunnel 0
        EAST(config-if)# ip address 172.16.12.2 255.255.255.252
        EAST(config-if)# tunnel source 10.2.2.1
        EAST(config-if)# tunnel destination 10.1.1.1 / <ip WEST>
        ```
    * Setting up routing through a GRE tunnel
        ```
        WEST(config)# router ospf 1
        WEST(config-router)# network 172.16.1.0 0.0.0.255 area 0
        WEST(config-router)# network 172.16.12.0 0.0.0.3 area 0
        ```

* Configuring IPv6 tunnels
    * Initial setup. The tunnel will be between router A and B via IPv4
        ```
        RouterA(config)# ipv6 unicast-routing
        
        RouterA(config)# interface fa0
        RouterA(config-if)# ipv6 address FEC0:0:0:1111::/64 eui-64
        
        RouterA(config)# interface fa1
        RouterA(config-if)# ip address 10.1.1.1 255.255.0.0
        ```
    * Configuring tunnel
        ```
        RouterA(config)# interface tunnel0
        RouterA(config-if)# no ip address
        RouterA(config-if)# ipv6 address FEC0:0:0:2222::1/124
        RouterA(config-if)# tunnel source fa1
        RouterA(config-if)# tunnel destination 10.1.1.2
        RouterA(config-if)# tunnel mode ipv6ip
        ```

* Dynamic VPN
    * Hub configuration
        - The `tunnel key NR` is only required when there is more than one mGRE interface on the router with the same tunnel source
        ```
        interface tunnel 0
        ip address 10.0.0.254 255.255.255.0
        tunnel source fa 0/0
        tunnel mode gre multipoint
        tunnel key 9999 (opcjonalnie)
        
        # Enabling nhrp on mgre interface
        ip nhrp network-id 10000
        ```
    * Spoke configuration
        ```
	    interface tunnel 0
		ip address 10.0.0.1 255.255.255.0
		tunnel source fa 0/0
		tunnel destination 172.16.0.254
		tunnel key 9999 # opcjonalnie
		ip nhrp network-id 10000

		# Spoke must register with hub
		ip nhrp nhs 10.0.0.254

		# Static mapping of address and NHS to physical address
		ip nhrp map 10.0.0.254 172.16.0.254
        ```