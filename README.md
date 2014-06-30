Route-Optimization-scheme-in-PMIPv6
===================================

###INTRODUCTION OF ROUTE OPTIMIZATION (RO) IN PMIPV6###

In PMIPv6, the packets generated between an MN and a CN are always transmitted via an LMA as RO is not supported. This increases the LMA load, and transmission delay increases, since the packets are transmitted via a longer path. Therefore, the RO is required to resolve the problem and to make the most efficient way to transmit the packets. P. Loureiro and M. Liebsch proposed a scheme to support the RO in PMIPv6. Figure 1 is the signaling procedure of an inter-domain handover in this scheme. When an MN attaches to an MAG1 domain, the MAG1 and the LMA1 make a bidirectional tunnel by handshaking the Proxy Binding Update (PBU) and Proxy Binding Acknowledgement (PBA) message. Next, when the first packet transmitted from the MN to the CN arrives at the LMA1, The LMA1 triggers the RO. The trigger message includes the MN-ID and the MAG1 address. LMA2, receiving the RO trigger message, performs the RO control function. LMA2 sends MAG2 the RO Init message, including the information of the MN and MAG1. MAG2, receiving the RO Init message exchanges the RO Setup and RO Setup Ack message with MAG1. The RO path is then established between MAG1 and MAG2. After that the packets are transmitted from the MN to the CN via the RO path.
 
![Alt text](http://monet.skku.ac.kr/img/open-source/Route-Optimization-1.gif "Fig. 1 Fig. 1. Signaling flow for RO") 
Fig. 1. Signaling flow for RO
 
###THE EXPLANNATION OF THE IMPLEMENTATION OF RO###
Base on the scheme of RO, The implementation of RO feature has 3 modules as describing below:
* RO Trigger: The module operates under network layer at LMA to recognize the communication between MN1 and MN2). Based on that, it determines to perform RO procedure. The module is green block in Fig. 2.
* RO Module: Operate on user space to exchange information with RO Trigger module. Based on that information, it controls the route and tunnel of communication between MNs. The module is yellow block in Fig. 2.
* RO Messages: There are some new messages in RO implementation. These messages are implemented in “Handler and Messages” modules or blue block in Fig. 2. These messages are listed below:
 + RO Trigger (ROT): send from LMA1 to LMA2.
 + RO Trigger Ack (ROTA): reply from LMA2 to LMA1
 + RO Init (ROI): send from LMA to MAG to order making RO between MNs.
 + RO Init Ack (ROIA): reply from MAG to LMA.
 + RO Setup (ROS): send from MAG to other MAG to request create an optimal path.
 + RO Setup Ack (ROSA): a response of ROS message
 
![Alt text](http://monet.skku.ac.kr/img/open-source/Route-Optimization-2.gif "Fig. 2. OAI PMIPv6 including RO feature.") 
Fig. 2 OAI PMIPv6 including RO feature.
 
###RO FEATURE TESTING###
We use “traceroute6” utility to test the RO feature in PMIPv6. Traceroute6 is used to show the route of IPv6 packets on the network. The Figs 3 and 4 show the route of packets delivered between MN1 and MN2 through PMIPv6 without RO feature and with RO feature supported. In Fig 3, we can see that the route of packets is via MN1, MAG, LMA, LMA2, MAG, and MN2. In contrast, Fig 4 show the optimal path of communication between the MN1 and MN2, the route of packets is via MN1, MAG1, MAG2, and MN2.
![Alt text](http://monet.skku.ac.kr/img/open-source/Route-Optimization-3.gif "Fig. 3. Route of Packets in PMIPv6 without RO feature") 
Fig. 3. Route of Packets in PMIPv6 without RO feature
![Alt text](http://monet.skku.ac.kr/img/open-source/Route-Optimization-4.gif "Fig. 4. Route of Packets in PMIPv6 with RO feature") 
Fig. 4. Route of Packets in PMIPv6 with RO feature
