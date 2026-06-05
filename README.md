
<h1>Small-Office-Home-Office-SOHO Design and Implementation</h1>
<h2>Technology use: VLAN, SVI and DCHP</h2>
<h2>Case Study</h2>
  <p>
Executive Custodian is a fast-growing company in Africa with Head Office  in Sydney. The company deals with selling commercial cleaning products, which are basically operated from the headquarters. The company is intending to open a branch in Newcastle. Thus, the company requires a Network Engineer to design the network for the branch. The network is intended to operate separately from the HQ network.
  </p>
<div>
  <h2>Tasks</h2>
  <p>Being a small network, the company has the following requirements for the design and implementation</p>
   <ol>
     <li>One router and one switch to be used (all CISCO products)</li>
     <li>3 departments (Admin/IT, Finance/HR and Customer service/Reception)</li>
     <li>Host devices in the network are required to obtain IPv4 address automatically</li>
     <li>Devices in all the departments are required to communicate with each other.</li>
     <li>Head Office has requested the Newcastle branch use the base network address <strong>192.168.1.0/24</strong></li>
   </ol>
</div>
<div>
<h1>Implemetation and Design</h1>
<h2>Topology</h2>
  <img src="images\SOHO TOPOLOGY.png" alt="Topology">
  <P>The base network address given was 192.168.1.0, and we need 3 subnets. The formular to use to know how many subnets needed is <strong>2<sup>n</sup></strong>, where N is the number of borrowed bits from the host sides. 

  <strong>1 1 0 0 0 0 0 0</strong> gives 192 subnet mask, and   2<sup>2</sup> = 4. So, we have 4 subnets.
  
  And to calculate the block size for subnet, we use <strong>256 - subnet mask (number of borrowed bits)</strong>

  So, we need 256 - 192 = 64 block size for each subnet.
  </P>
  
<table>
  <tr>
    <th></th>
    <th>Subnet 1</th>
    <th>Subnet 2</th>
    <th>Subnet 3</th>
    <th>Subnet 4</th>
  </tr>
   <tr>
    <td>Network Address</td>
     <td><strong>192.168.1.0</strong></td>
     <td><strong>192.168.1.64</strong></td>
     <td><strong>192.168.1.128</strong></td>
     <td><strong>192.168.1.192</strong></td>
   </tr>
  <tr>
    <td>First Usable IP</td>
    <td>192.168.1.1</td>
    <td>192.168.1.65</td>
    <td>192.168.1.129</td>
    <td>192.168.1.193</td>
  </tr>
  <tr>
    <td>Last Usable IP</td>
    <td>192.168.1.62</td>
    <td>192.168.1.126</td>
    <td>192.168.1.190</td>
    <td>192.168.1.254</td>
  </tr>
    <tr>
    <td>Broadcast Address</td>
    <td>192.168.1.63</td>
    <td>192.168.1.127</td>
    <td>192.168.1.191</td>
    <td>192.168.1.255</td>
  </tr>
  
</table>
</div>
<div>
  <h3>Configuration</h3>
  <details>
    <summary>Router</summary>
    enable
conf t

int fa0/0

no shut

exit

int fa0/0.1

encapsulation dot1q 10

ip address 192.168.1.1 255.255.255.192

no shut

exit


int fa0/0.2

encapsulation dot1q 20

ip address 192.168.1.65 255.255.255.192

no shut

exit



int fa0/0.3

encapsulation dot1q 30

ip address 192.168.1.129 255.255.255.192

no shut

exit


service dhcp

ip dhcp pool Admin/IT

network 192.168.1.0 255.255.255.192

default-router 192.168.1.1

dns-server 192.168.1.1

domain-name adminit.com

exit

ip dhcp pool Finance/HR

network 192.168.1.64 255.255.255.192

default-router 192.168.1.65

dns-server 192.168.1.65

domain-name financehr.com

exit

ip dhcp pool CS/recept

network 192.168.1.128 255.255.255.192

default-router 192.168.1.129

dns-server 192.168.1.129

domain-name csrcept

exit

  </details>
  <details>
    <summary>Switch</summary>
    
enable
    
conf t

int range e0/1-3, e1/0-3, e2/0-3

switchport mode access

exit


vlan 10

name Admin/IT

exit

int range e0/1-3, e1/0

switchport access vlan 10

exit


vlan 20

name Finance/Hr

exit

int range e1/1-3, e2/0

switchport access vlan 20

exit

vlan 30

name CS/Recpt

exit

int range e2/1-3

switchport access vlan 30

exit

int e0/0

switchport trunk encapsulation dot1q

switchport mode trunk

exit
  </details>

  <details>
    <summary>All the computers</summary>
    ip dhcp
  </details>
</div>
<div>
  <!--This is great-->
  <h3> In conclussion</h3>
  <p>All the PCs in each department are able to get ip addresses from the DHCP server using their default gateway configured.
  PCs in different Vlans can communicate with each other too. In the pictures below, computers in Finance Department and Customer Service are able to get ip addresses from DHCP server and it shows shows PC in Finance communicating with PC in Customer Service.
  </p>
  <h4>Topology</h4>
  <img src="images\ping and dhcp.png" alt="ping">
  <p>  
  
  </p>
  <img src="images\Admin PC.png" alt="ping">
</div>
  
 

