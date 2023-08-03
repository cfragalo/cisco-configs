
# CISCO - Configuring Terminal server 




<br>

```
hostname <HOSTNAME>

!
!

ip host PUERTO_2066 2066 10.0.0.10
ip host PUERTO_2067 2067 10.0.0.10
ip host PUERTO_2068 2068 10.0.0.10
ip host PUERTO_2069 2069 10.0.0.10
ip host PUERTO_2070 2070 10.0.0.10
ip host PUERTO_2071 2071 10.0.0.10
ip host PUERTO_2072 2072 10.0.0.10
ip host PUERTO_2073 2073 10.0.0.10
ip host PUERTO_2074 2074 10.0.0.10
ip host PUERTO_2075 2075 10.0.0.10
ip host PUERTO_2076 2076 10.0.0.10
ip host PUERTO_2077 2077 10.0.0.10
ip host PUERTO_2078 2078 10.0.0.10
ip host PUERTO_2079 2079 10.0.0.10
ip host PUERTO_2080 2080 10.0.0.10
ip host PUERTO_2081 2081 10.0.0.10

!   
!

interface Loopback0
 description -- Local MGMT Interface --
 ip address 10.0.0.10 255.255.255.255

!
!

menu consolas title ^C


Bienvenido al servidor de consolas ---  ESNOG-28
___________________________________________________________________________

NODO: ESNOG-28 

DATACENTER: UPC
LOCATION: Edifici Vertex (Placa d'Eusebi Guell, 08034 Barcelona)

^C
menu consolas prompt ^CCTeclee un numero para seleccionar una opcion:^C
menu consolas text 0 salir a <HOSTNAME>
menu consolas command 0 menu-exit
menu consolas text l<no> Limpia la sesion por numero (p.ejemplo): l5
menu consolas text 1 FREE: PUERTO_2066
menu consolas command 1 resume PUERTO_2066 /connect telnet PUERTO_2066
menu consolas text 2 FREE: PUERTO_2067
menu consolas command 2 resume PUERTO_2067 /connect telnet PUERTO_2067
menu consolas text 3 FREE: PUERTO_2068
menu consolas command 3 resume PUERTO_2068 /connect telnet PUERTO_2068
menu consolas text 4 FREE: PUERTO_2069
menu consolas command 4 resume PUERTO_2069 /connect telnet PUERTO_2069
menu consolas text 5 FREE: PUERTO_2070
menu consolas command 5 resume PUERTO_2070 /connect telnet PUERTO_2070
menu consolas text 6 FREE: PUERTO_2071
menu consolas command 6 resume PUERTO_2071 /connect telnet PUERTO_2071
menu consolas text 7 FREE: PUERTO_2072
menu consolas command 7 resume PUERTO_2072 /connect telnet PUERTO_2072
menu consolas text 8 FREE: PUERTO_2073
menu consolas command 8 resume PUERTO_2073 /connect telnet PUERTO_2073
menu consolas text 9 FREE: PUERTO_2076 
menu consolas command 9 resume PUERTO_2074 /connect telnet PUERTO_2074
menu consolas text 10 FREE: PUERTO_2075
menu consolas command 10 resume PUERTO_2075 /connect telnet PUERTO_2075
menu consolas text 11 FREE: PUERTO_2076
menu consolas command 11 resume PUERTO_2076 /connect telnet PUERTO_2076
menu consolas text 12 FREE: PUERTO_2077
menu consolas command 12 resume PUERTO_2077 /connect telnet PUERTO_2077
menu consolas text 13 FREE: PUERTO_2078
menu consolas command 13 resume PUERTO_2078 /connect telnet PUERTO_2078
menu consolas text 14 FREE: PUERTO_2079
menu consolas command 14 resume PUERTO_2079 /connect telnet PUERTO_2079
menu consolas text 15 FREE: PUERTO_2080
menu consolas command 15 resume PUERTO_2080 /connect telnet PUERTO_2080
menu consolas text 16 FREE: PUERTO_2081
menu consolas command 16 resume PUERTO_2081 /connect telnet PUERTO_2081
menu consolas command l1 clear line tty 66
menu consolas command l2 clear line tty 67
menu consolas command l3 clear line tty 68
menu consolas command l4 clear line tty 69
menu consolas command l5 clear line tty 70
menu consolas command l6 clear line tty 71
menu consolas command l7 clear line tty 72
menu consolas command l8 clear line tty 73
menu consolas command l9 clear line tty 74
menu consolas command l10 clear line tty 75
menu consolas command l11 clear line tty 76
menu consolas command l12 clear line tty 77
menu consolas command l13 clear line tty 78
menu consolas command l14 clear line tty 79
menu consolas command l15 clear line tty 80
menu consolas command l16 clear line tty 81
menu consolas clear-screen
menu consolas status-line
menu consolas default 20
menu consolas line-mode

```

<br>


```

--- Console Port

line con 0
 exec-timeout 30 0
 logging synchronous
 login local


--- Aux Port

line aux 0
 exec-timeout 30 0
 logging synchronous
 no exec
 transport input all
 transport output all
 stopbits 1


--- In case you use NM-16A card

line 1/1 1/15
exec-timeout 30 0
logging synchronous
no exec
transport input all
transport output all


--- In case you use NM-32A card 

line 1/0 1/31
 exec-timeout 30 0
 logging synchronous
 no exec
 transport input all
 transport output all


--- VTY lines

line vty 0 4
 exec-timeout 15 0
 logging synchronous
 login local
 autocommand menu consolas    # If we're like to show up the menu at first 
 transport input ssh
 transport output ssh
line vty 5 15
 exec-timeout 15 0
 logging synchronous
 login local
 autocommand menu consolas    # If we're like to show up the menu at first
 transport input ssh
 transport output ssh

```

<br>

In case you need to change the speed or other parametes you can do it:

```
--- For example, port 3 ("PUERTO_2068"):


hostname-rt-oob(config)#configure terminal

hostname-rt-oob(config-line)#speed ?
  <0-4294967295>  Transmit and receive speeds


hostname-rt-oob(config-line)#stopbits ?
  1    One stop bit
  1.5  One and one-half stop bits
  2    Two stop bits


hostname-rt-oob(config-line)#  flowcontrol ?
  NONE      Set no flow control
  hardware  Set hardware flow control
  software  Set software flow control


--- Final config:

line 1/3
 exec-timeout 30 0
 logging synchronous
 no exec
 transport input all
 transport output all
 stopbits 1
 speed 115200
 flowcontrol hardware

```

<br>

## Security (allow access)

<br>

we can define standard access-list to allow remote access to these console ports: 

```

ip access-list standard ALLOWED_HOSTS
 permit 10.0.0.10                 # localhost
 permit 3.3.3.3                   # Public Cloud - Bastion Server
 permit 172.16.0.0 0.15.255.255   # Internal MGMT network


line 1/5                          # Apply on all the lines we need it
 access-class ALLOWED_HOSTS in

```

<br>

## Menu consolas:

<br>

Final Result when we log in to the router:

<br>

![OOB_menu_consolas](/images/OOB_menu_consolas.png)

<br>

## Links:

<br>

[Understanding 16- and 32-Port Async Network Modules](https://www.cisco.com/c/en/us/support/docs/routers/3600-series-multiservice-platforms/7258-hw-async.html) - Cisco Systems

[Configuting a Terminal/Comm Server](https://www.cisco.com/c/en/us/support/docs/dial-access/asynchronous-connections/5466-comm-server.html) - Cisco Systems 

[Configure Cable Requirements for Console and AUX Ports](https://www.cisco.com/c/en/us/support/docs/routers/7000-series-routers/12223-14.html) - Cisco Systems

<br>

## Some Pictures:

<br>



<br>

Cisco 2811 with NM-16A Card + CAB-Octal-Async

<br>

![OOB_cisco_NM16A_rear](/images/OOB_cisco_NM16A_rear.jpg)

<br>

<br>

Best practise, should be add patch pannel to connect all console ports

Async Patch Pannel configuration: 

- 1 x Keystone Patch Pannel (16 ports)
- 16 x Keystone RJ45-RJ45 Adapter

<br>

![OOB_async_patch](/images/OOB_cisco_patch_pannel.png)



