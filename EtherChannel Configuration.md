# Configure EtherChannel
<a href="https://imgbb.com/"><img src="https://i.ibb.co/d57t2dV/Topologi.png" alt="Topologi" border="0"></a>

## Objectives
Part 1: Configure Basic Switch Settings

Part 2: Configure an EtherChannel with Cisco PAgP

Part 3: Configure an 802.3ad LACP EtherChannel

Part 4: Configure a Redundant EtherChannel Link


## Port Channel table
<a href="https://ibb.co/1qSzx8K"><img src="https://i.ibb.co/WyTV1gF/Table.png" alt="Table" border="0"></a>

## Part 1: Configure Basic Switch Settings
### Step 1: Configure Port Channel 1.
a. created for this activity aggregates ports F0/21 and F0/22 between S1 and S3. Configure the ports on both switches as static trunk ports

#### S1
        Switch(config)#hostname S1
        S1(config)#interface range f0/21-22
        S1(config-if-range)#switchport mode trunk
        S1(config-if-range)#interface range g0/1-2
        S1(config-if-range)#switchport mode trunk

#### S3
        S3(config)#interface range f0/21-22
        S3(config-if-range)#switchport mode trunk
        S3(config)#interface range f0/23-24
        S3(config-if-range)#switchport mode trunk

b.  ensure that you have an active trunk link for those two links, and the native VLAN on both links is the same
        S1#show interface trunk

c. On S1 and S3, add ports F0/21 and F0/22 to Port Channel 1 with mode desirable

#### S1
        S1(config)#interface range f0/21-22
        S1(config-if-range)#shutdown
        S1(config-if-range)#channel-group 1 mode desirable
        S1(config-if-range)#no shutdown

#### S2
        S3(config)#interface range f0/21-22
        S3(config-if-range)#shutdown
        S3(config-if-range)#channel-group 1 mode desirable
        S3(config-if-range)#no shutdown

d. Configure the logical interface to become a trunk

#### S1

        S1(config)#interface port-channel 1
        S1(config-if)#switchport mode trunk

#### S3

        S3(config)#interface port-channel 1
        S3(config-if)#switchport mode trunk

### Step 2: Verify Port Channel 1 status on S1 and S3
        S1#show etherchannel summary
        Flags:  D - down        P - in port-channel
                I - stand-alone s - suspended
                H - Hot-standby (LACP only)
                R - Layer3      S - Layer2
                U - in use      f - failed to allocate aggregator
                u - unsuitable for bundling
                w - waiting to be aggregated
                d - default port


        Number of channel-groups in use: 1
        Number of aggregators:           1

        Group  Port-channel  Protocol    Ports
        ------+-------------+-----------+----------------------------------------------

        1      Po1(SU)           PAgP   Fa0/21(P) Fa0/22(P) 


## Part 3: Configure an 802.3ad LACP EtherChannel
### Step 1: Configure Port Channel 2 between S1 and S2
 #### S1
        S1(config)#interface range g0/1-2
        S1(config-if-range)#shutdown
        S1(config-if-range)#channel-group 2 mode active
        S1(config-if-range)#no shutdown
        S1(config)#interface port-channel 2
        S1(config-if)#switchport mode trunk

 #### S2
        Switch(config)#interface range g0/1-2
        Switch(config-if-range)#shutdown
        Switch(config-if-range)#channel-group 2 mode active
        Switch(config-if-range)#no shutdown
        Switch(config)#interface port-channel 2
        Switch(config-if)#switchport mode trunk


### Step 2 Veify port channel 2 status

        S1#show etherchannel summary
        Flags:  D - down        P - in port-channel
                I - stand-alone s - suspended
                H - Hot-standby (LACP only)
                R - Layer3      S - Layer2
                U - in use      f - failed to allocate aggregator
                u - unsuitable for bundling
                w - waiting to be aggregated
                d - default port


        Number of channel-groups in use: 2
        Number of aggregators:           2

        Group  Port-channel  Protocol    Ports
        ------+-------------+-----------+----------------------------------------------

        1      Po1(SU)           PAgP   Fa0/21(P) Fa0/22(P) 
        2      Po2(SU)           LACP   Gig0/1(P) Gig0/2(P) 



## Part 4: Configure a Redundant EtherChannel Link
### Step 1: Configure Port Channel 3
        S2(config)#interface range f0/23-24
        S2(config-if-range)#channel-group 3 mode ?
        active     Enable LACP unconditionally
        auto       Enable PAgP only if a PAgP device is detected
        desirable  Enable PAgP unconditionally
        on         Enable Etherchannel only
        passive    Enable LACP only if a LACP device is detected

a.  On switch S2, add ports F0/23 and F0/24 to Port Channel 3 with the channel-group 3 mode passive command. The passive option indicates that you want the switch to use LACP only if another LACP device is detected. Statically configure Port Channel 3 as a trunk interface.

#### S2
        S2(config)#interface range f0/23-24
        S2(config-if-range)#shutdown
        S2(config-if-range)#channel-group 3 mode passive
        S2(config-if-range)#no shutdown
        S2(config-if-range)#exit
        S2(config)#interface port-channel 3
        S2(config-if)#switchport mode trunk

b On S3, add ports F0/23 and F0/24 to Port Channel 3 with  mode active

#### S3
        S3(config)#interface range f0/23-24
        S3(config-if-range)#shutdown
        S3(config-if-range)#channel-group 3 mode active
        S3(config-if-range)#no shutdown
        S3(config-if-range)#interface port-channel 3
        S3(config-if)#switchport mode trunk

### Step 2: Verify Port Channel 3 status.
 a veify
        #show etherchannel summary
        Flags:  D - down        P - in port-channel
                I - stand-alone s - suspended
                H - Hot-standby (LACP only)
                R - Layer3      S - Layer2
                U - in use      f - failed to allocate aggregator
                u - unsuitable for bundling
                w - waiting to be aggregated
                d - default port


        Number of channel-groups in use: 2
        Number of aggregators:           2

        Group  Port-channel  Protocol    Ports
        ------+-------------+-----------+----------------------------------------------

        2      Po2(SU)           LACP   Gig0/1(P) Gig0/2(P) 
        3      Po3(SD)           LACP   Fa0/23(I) Fa0/24(I)

b .Creating EtherChannel links does not prevent Spanning Tree from detecting switching loops. View the spanning tree status of the active ports on S1.

        S1#show spanning-tree active
        VLAN0001
        Spanning tree enabled protocol ieee
        Root ID    Priority    32769
                    Address     0001.436E.8494
                    Cost        9
                    Port        27(Port-channel1)
                    Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec

        Bridge ID  Priority    32769  (priority 32768 sys-id-ext 1)
                    Address     000A.F313.2395
                    Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec
                    Aging Time  20

        Interface        Role Sts Cost      Prio.Nbr Type
        ---------------- ---- --- --------- -------- --------------------------------
        Po2              Desg FWD 3         128.28   Shr
        Po1              Root FWD 9         128.27   Shr


Port Channel 2 is not operative because Spanning Tree Protocol placed some ports into blocking mode. Unfortunately, those ports were the Gigabit ports. In this topology, you can restore these ports by configuring S1 to be primary root for VLAN 1. You could also set the priority to 24576.


        S1(config)#spanning-tree vlan 1 root primary

        or 
        
        S1(config)# spanning-tree vlan 1 priority 24576















