# Implement DHCPv4

<a href="https://imgbb.com/"><img src="https://i.ibb.co/CJz1kSD/Topologi.png" alt="Topologi" border="0"></a>

## VLAN Table

<a href="https://ibb.co/vVb61W8"><img src="https://i.ibb.co/PFRvxy8/VLAN-Table.png" alt="VLAN-Table" border="0"></a>
## Objectives
Part 1: Build the Network and Configure Basic Device Settings
Part 2: Configure and verify two DHCPv4 Servers on R1
Part 3: Configure and verify a DHCP Relay on R2


## Part 1: Build the Network and Configure Basic Device Settings
### Step 1: Establish an addressing scheme
Subnet the network 192.168.1.0/24 to meet the following requirements:
a. One subnet, “Subnet A”, supporting 58 hosts (the client VLAN at R1).
Subnet A: 192.168.1.0/26

Record the first IP address in the Addressing Table for R1 G0/0/1.100.
b. One subnet, “Subnet B”, supporting 28 hosts (the management VLAN at R1).
Subnet B:192.168.1.0/27

Record the first IP address in the Addressing Table for R1 G0/0/1.200. Record the second IP address in
the Address Table for S1 VLAN 200 and enter the associated default gateway.
c. One subnet, “Subnet C”, supporting 12 hosts (the client network at R2).
Subnet C: 192.168.1.0/28

## Subnetting IP

<a href="https://ibb.co/5rHz0G3"><img src="https://i.ibb.co/bs9y0QV/Addressing-Table.png" alt="Addressing-Table" border="0"></a><br /><a target='_blank' href='https://imgbb.com/'>gif 1 1</a><br />

### Step 3: Configure basic settings for each router.

a. Assign a device name to the router.
b. Disable DNS lookup 
c. Assign ccna@123 as the privileged EXEC encrypted password.
d. Assign ccna@321 as the console password and enable login.
e. Assign ccna@321 as the VTY password and enable login.
f. Encrypt the plaintext passwords.
g. Create a banner 
h. Save configuration 
i. Set the clock on the router 

        Router(config)#hostname R1
        R1(config)#no ip domain-lookup
        R1(config)#enable secret ccna@123
        R1(config)#line console 0
        R1(config-line)#password ccna@321
        R1(config-line)#login
        R1(config-line)#exit
        R1(config)#line vty 0 4
        R1(config-line)#password ccna@321
        R1(config-line)#login
        R1(config-line)#exit
        R1(config)#service password-encryption
        R1(config)#banner motd #selamat datang admin!#
        R1(config)#end
        R1#copy running-config startup-config
        R1#clock set 14:00:00 08 August 2022


### Step 4: Configure basic settings for each switch.

a. Assign a device name to the switch.
b. Disable DNS lookup 
c. Assign ccna@123 as the privileged EXEC encrypted password.
d. Assign ccna@321 as the console password and enable login.
e. Assign ccna@321 as the VTY password and enable login.
f. Encrypt the plaintext passwords.
g. Create a banner 
h. Save  configuration 
i. Set the clock on the switch 
j. Copy the running configuration to the startup configuration

#### S1
        Switch(config)#hostname S1
        S1(config)#no ip domain-lookup
        S1(config)#enable secret ccna@123
        S1(config)#line console 0
        S1(config-line)#password ccna@321
        S1(config-line)#login
        S1(config-line)#exit
        S1(config)#line vty 0 15
        S1(config-line)#password ccna@321
        S1(config-line)#login
        S1(config-line)#exit
        S1(config)#service password-encryption
        S1(config)#banner motd # Selamat datang admin!#
        S1(config)#end
        S1#copy running-config startup-config
        S1#clock set 14:30:00 08 August 2022
        S1#

#### S2
        Switch(config)#hostname S2
        S2(config)#no ip domain-lookup
        S2(config)#enable secret ccna@123
        S2(config)#line console 0
        S2(config-line)#password ccna@321
        S2(config-line)#login
        S2(config-line)#exit
        S2(config)#line vty 0 15
        S2(config-line)#password ccna@321
        S2(config-line)#login
        S2(config-line)#exit
        S2(config)#service password-encryption
        S2(config)#banner motd # Selamat datang admin!#
        S2(config)#end
        S2#
        S2#copy running-config startup-config
        S2#clock set 14:50:00 08 August 2022


### Step 5: Create VLANs on S1.
Note: S2 is only configured with basic settings.

a. Create and name the required VLANs on switch 1 from the table above.

        S1(config)#vlan 100
        S1(config-vlan)#name Clients
        S1(config-vlan)#vlan 200
        S1(config-vlan)#name Management
        S1(config-vlan)#vlan 999    
        S1(config-vlan)#name parking_lot
        S1(config-vlan)#vlan 1000
        S1(config-vlan)#name Native
        S1(config-vlan)#exit


b. Configure and activate the management interface on S1 (VLAN 200) using the second IP address from
the subnet calculated earlier. Additionally, set the default gateway on S1.

        S1(config)#interface vlan 200
        S1(config-if)#ip address 192.168.1.66 255.255.255.224
        S1(config-if)#no shutdown
        S1(config-if)#exit
        S1(config)#ip default-gateway 192.168.1.65

c. Configure and activate the management interface on S2 (VLAN 1) using the second IP address from the
subnet calculated earlier. Additionally, set the default gateway on S2

        S2(config)#interface vlan 200
        S2(config-if)#ip address 192.168.1.98 255.255.255.240
        S2(config-if)#no shutdown
        S2(config-if)#exit
        S2(config)#ip default-gateway 192.168.1.97

d. Assign all unused ports on S1 to the Parking_Lot VLAN, configure them for static access mode, and
administratively deactivate them. On S2, administratively deactivate all the unused ports.

#### S1
        S1(config)#interface range f0/1-4, f0/7-24, g0/1-2
        S1(config-if-range)#switchport mode access
        S1(config-if-range)#switchport access vlan 999
        S1(config-if-range)#shutdown

#### S2
        S2(config)#interface range f0/1-4, f0/6-17,f0/19-24, g0/1-2
        S2(config-if-range)#switchport mode access
        S2(config-if-range)#shutdown


### Step 6: Assign VLANs to the correct switch interfaces.

a. Assign used ports to the appropriate VLAN (specified in the VLAN table above) and configure them for static access mode.

        S1(config)#interface f0/6
        S1(config-if)#switchport mode access
        S1(config-if)#switchport access vlan 100

b. Verify that the VLANs are assigned to the correct interfaces.

        VLAN Name                             Status    Ports
        ---- -------------------------------- --------- -------------------------------
        1    default                          active    Fa0/5
        100  Clients                          active    Fa0/6
        200  Management                       active    
        999  parking_lot                      active    Fa0/1, Fa0/2, Fa0/3, Fa0/4
                                                        Fa0/7, Fa0/8, Fa0/9, Fa0/10
                                                        Fa0/11, Fa0/12, Fa0/13, Fa0/14
                                                        Fa0/15, Fa0/16, Fa0/17, Fa0/18
                                                        Fa0/19, Fa0/20, Fa0/21, Fa0/22
                                                        Fa0/23, Fa0/24, Gig0/1, Gig0/2
        1000 Native                           active    
        1002 fddi-default                     active    
        1003 token-ring-default               active    
        1004 fddinet-default                  active    
        1005 trnet-default                    active    
        S1#

Question:
Why is interface F0/5 listed under VLAN 1?
Port 5 is in the default VLAN and has not been configured as an 802.1Q trunk.

### Step 7: Manually configure S1’s interface F0/5 as an 802.1Q trunk.
a. Change the switchport mode on the interface to force trunking.
        S1(config)#interface f0/5
        S1(config-if)#switchport mode trunk

b. As a part of the trunk configuration, set the native VLAN to 1000.

        S1(config-if)#switchport trunk native vlan 1000

c. As another part of trunk configuration, specify that VLANs 100, 200, and 1000 are allowed to cross the trunk.

        S1(config-if)#switchport trunk allowed vlan 100,200,1000
        S1(config-if)#end

d. Save the running configuration to the startup configuration file.

         S1#copy running-config startup-config

e. Verify trunking status.

S1#show interface trunk
        Port        Mode         Encapsulation  Status        Native vlan
        Fa0/5       on           802.1q         trunking      1000

        Port        Vlans allowed on trunk
        Fa0/5       100,200,1000

        Port        Vlans allowed and active in management domain
        Fa0/5       100,200,1000

        Port        Vlans in spanning tree forwarding state and not pruned
        Fa0/5       100,200,1000


### Step 8: Configure Inter-VLAN Routing on R1
a. Activate interface G0/0/1 on the route
        R1(config)#interface g0/0/1
        R1(config-if)#no shutdown

b.Configure sub-interfaces for each VLAN as required by the IP addressing table

        R1(config-subif)#description Client Network
        R1(config-subif)#encapsulation dot1q 100
        R1(config-subif)#ip address 192.168.1.1 255.255.255.192
        R1(config-subif)#interface g0/0/1.200
        R1(config-subif)#description Management Network
        R1(config-subif)#encapsulation dot1q 200
        R1(config-subif)#ip address 192.168.1.65 255.255.255.224
        R1(config-subif)#interface g0/0/1.1000
        R1(config-subif)#encapsulation dot1q 1000
        R1(config-subif)#description Native VLAN

c. Verify the sub-interfaces are operational.

        R1#show ip interface brief
        Interface                       IP-Address      OK? Method Status                Protocol 
        GigabitEthernet0/0/0            unassigned      YES unset  administratively down down 
        GigabitEthernet0/0/1            unassigned      YES unset  up                    up 
        GigabitEthernet0/0/1.100        192.168.1.1     YES manual up                    up 
        GigabitEthernet0/0/1.200        192.168.1.65    YES manual up                    up 
        GigabitEthernet0/0/1.1000       unassigned      YES unset  up                    up 
        Vlan1                           unassigned      YES unset  administratively down down

### Step 9: Configure G0/0/1 on R2, then G0/0/0 and static routing for both routers
a. Configure G0/0/1 on R2 with the first IP address of Subnet C 

        Router(config)#hostname R2
        R2(config)#interface g0/0/1
        R2(config-if)#ip address 192.168.1.97 255.255.255.240
        R2(config-if)#no shutdown

b. Configure interface G0/0/0 for each router based on the IP Addressing table above.

### R1
        R1(config)#interface g0/0/0
        R1(config-if)#ip address 10.0.0.1 255.255.255.252
        R1(config-if)#no shutdown
#### R2
        R2(config)#interface g0/0/0
        R2(config-if)#ip address 10.0.0.2 255.255.255.252
        R2(config-if)#no shutdown
c. Configure a default route on each router pointed to the IP address of G0/0/0 on the other router.

#### R1

    R1(config)#ip route 0.0.0.0 0.0.0.0 10.0.0.2

#### R2

    R2(config)#ip route 0.0.0.0 0.0.0.0 10.0.0.1

d. Verify static routing is working by pinging R2’s G0/0/1 address from R1.

e. Save the running configuration to the startup configuration file.

        R1#copy running-config startup-config

## Part 2: Configure and verify two DHCPv4 Servers on R1
The DHCPv4 server will service two subnets,
Subnet A and Subnet C

### Step 1: Configure R1 with DHCPv4 pools for the two supported subnets. Only the DHCP Pool for
subnet A is given below
a. Exclude the first five useable addresses from each address pool.
Open configuration window
b. Create the DHCP pool (Use a unique name for each pool).
c. Specify the network that this DHCP server is supporting.
d. Configure the domain name as ccna-lab.com
e. Configure the appropriate default gateway for each DHCP pool.
f. Configure the lease time for 2 days 12 hours and 30 minutes.

        R1(config)#ip dhcp excluded-address 12.168.1.1 192.168.1.5
        R1(config)#ip dhcp pool R1_Client_LAN
        R1(dhcp-config)#network 192.168.1.0 255.255.255.192
        R1(dhcp-config)#domain-name ccna-lab.com
        R1(dhcp-config)#default-router 192.168.1.1
        R1(dhcp-config)#lease 2 12 30

        
g. Next, configure the second DHCPv4 Pool using the pool name R2_Client_LAN and the calculated
network, default-router and use the same domain name and lease time from the previous DHCP pool.

       
        R2(config)#ip dhcp excluded-address 192.168.1.97 192.168.1.101
        R2(config)#ip dhcp pool R2_Client_LAN
        R2(dhcp-config)#network 192.168.1.96 255.255.255.240
        R2(dhcp-config)#default-router 12.168.1.97
        R2(dhcp-config)#domain-name ccna-lab.com
        R2(dhcp-config)#lease 2 12 30
      

### Step 2: Save your configuration
Save the running configuration to the startup configuration file.

Step 3: Verify the DHCPv4 Server configuration
a. show ip dhcp pool to examine the pool details.
b. Issue the command show ip dhcp bindings to examine established DHCP address assignments.
c. Issue the command show ip dhcp server statistics to examine DHCP messages.
Step 4: Attempt to acquire an IP address from DHCP on PC-A
a. Open a command prompt on PC-A and issue the command ipconfig /renew.
b. Once the renewal process is complete, issue the command ipconfig to view the new IP information.
c. Test connectivity by pinging R1’s G0/0/1 interface IP address.

## Part 3: Configure and verify a DHCP Relay on R2

### Step 1: Configure R2 as a DHCP relay agent for the LAN on G0/0/1
a. Configure the ip helper-address command on G0/0/1 specifying R1’s G0/0/0 IP address.
Open configuration window

        R2(config)#interface g0/0/1
        R2(config-if)#ip helper-address 10.0.0.1


b. Save your configuration.

        R2# copy running-config startup-config

Step 2: Attempt to acquire an IP address from DHCP on PC-B
a. Open a command prompt on PC-B and issue the command ipconfig /renew.
b. Once the renewal process is complete, issue the command ipconfig to view the new IP information.
c. Test connectivity by pinging R1’s G0/0/1 interface IP address.
d. Issue the show ip dhcp binding on R1 to verify DHCP bindings.
e. Issue the show ip dhcp server statistics on R1 and R2 to verify DHCP messages





































