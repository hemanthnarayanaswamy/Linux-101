# The `ping` command

The [ping](https://linuxize.com/post/linux-ping-command/) command is one of the most used tools for troubleshooting, testing, and diagnosing network connectivity issues.

_Ping_ works by sending one or more _ICMP (Internet Control Message Protocol)_ Echo Request packets to a specified destination IP on the network and waiting for a reply. When the destination receives the packet, it responds with an ICMP echo reply.

With the ping command, you can determine whether a remote destination IP is active or inactive. You can also find the round-trip delay in communicating with the destination and check whether there is a packet loss.

If ping does not return a reply, it means that the network communication is not established. When this happens, it does not always mean that the destination IP is not active. Some hosts may have a firewall that is blocking the ICMP traffic or set to not respond to ping requests.

```bash
ping -c 2 www.geeksforgeeks.org # send 2 ping requests

ping -s 40 -c 5 www.geeksforgeeks.org  # send packets of size 40 bytes, and 5 ping requests

ping -i 2 www.geeksforgeeks.org # set interval to 2 seconds

ping -c 5 -q www.geeksforgeeks.org # show summary for 5 packets

ping -w 3 www.geeksforgeeks.org # set ping timeout

#-f: Enables flood ping mode
# sends packets as fast as possible. Useful for stress testing network perfromance
ping -f www.geeksforgeeks.org
```
