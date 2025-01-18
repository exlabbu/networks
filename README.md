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
        Router(config)# banner motd $Wir mussen die Juden ausrotten$
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
    * DNAT (Dynamic NAT)
        ```
        Router(config)# access-list <number> permit <ip_addres> <wildcard>
        Router(config)# ip nat inside source list <number> pool <pool_name>
        ```
    * PAT
        ```
        Router(config)# access-list <number> permit <ip_addres> <wildcard>
        Router(config)# ip nat inside source list <number> <interface <port>|pool <pool_name>> overload
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
            - 0 -> emergencies
	        - 7 -> debugging
            ```
	        Router(config)# logging trap ? [0-7]
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
        - utworzenie class-map, ktore bedzie zezwalal na dany ruch z wewnatrz do internetu match-any -> wystarczy, aby tylko jeden z poniższych matchy pasował. Dla tego przykladu wyjdzie nam TCP or UDP or ICMP
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
