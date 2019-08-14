# Vagrant test environment

This branch contains a test environment for the dhcp role, powered by Vagrant and VirtualBox. Base boxes are from the [Bento project](http://chef.github.io/bento/).

I use [git-worktree(1)](https://git-scm.com/docs/git-worktree) to include the test code into the working directory. Instructions for running the tests:

1. Go to the root directory of your local working copy of the repository. Fetch the test branch: `git fetch origin vagrant-tests`
2. Create a Git worktree for the test code: `git worktree add vagrant-tests vagrant-tests` (remark: this requires at least Git v2.5.0). This will create a directory `vagrant-tests/`.
3. `cd vagrant-tests/`

## Test setup

The test environment contains VMs for each supported platform. Use `vagrant status` to get a list:

```console
$ vagrant status
Current machine states:

srvcentos                 not created (virtualbox)
srvfedora                 not created (virtualbox)
srvubuntu                 not created (virtualbox)
```

**WARNING!** Be aware that **only a single VM** should be running at the same time! Each VM is configured identically, so you would have multiple DHCP servers with the same network settings on the same subnet, which may result in unexpected behaviour. Run `vagrant up srvDISTRO` to boot the chosen VM and run the [test playbook](test.yml).

At this time, no automated functionality tests are implemented. Suggestions for manual testing:

- After booting, the DHCP server should have IP address 192.168.222.2/24. This network interface should be attached to a VirtualBox Host Only interface. In the examples below, we assume the interface is named `vboxnet2`.
- Open a console and follow the system logs to see what is going on: `sudo journalctl -f -l -u dhcpd.service`
- You can use nmap from the host system to send a DHCPDISCOVER request on the subnet with the DHCP server to check if it works:

    ```console
    $ sudo nmap -e vboxnet2 --script broadcast-dhcp-discover
    Starting Nmap 7.70 ( https://nmap.org ) at 2019-08-14 21:25 CEST
    Pre-scan script results:
    | broadcast-dhcp-discover:
    |   Response 1 of 1:
    |     IP Offered: 192.168.222.50
    |     DHCP Message Type: DHCPOFFER
    |     Server Identifier: 192.168.222.2
    |     IP Address Lease Time: 5m00s
    |     Subnet Mask: 255.255.255.0
    |     Domain Name Server: 10.0.2.3, 10.0.2.4
    |     Domain Name: example.com
    |_    Broadcast Address: 192.168.222.255
    WARNING: No targets were specified, so 0 hosts scanned.
    Nmap done: 0 IP addresses (0 hosts up) scanned in 1.23 seconds
    ```

    The logs should show that the request was received and a DHCPOFFER was sent to the client:

    ```console
    Aug 14 19:25:29 srvcentos dhcpd[3978]: DHCPDISCOVER from de:ad:c0:de:ca:fe via eth1
    Aug 14 19:25:30 srvcentos dhcpd[3978]: DHCPOFFER on 192.168.222.50 to de:ad:c0:de:ca:fe via eth1
    ```
- You can also create a new VirtualBox VM with a network interface attached to the same Host-Only interface as the DHCP server. To check if reserved IP addresses based on MAC address works, you can set the VM's MAC address to:
    - `de:ad:c0:de:ca:fe`, IP address 192.168.222.150
    - `00:de:ad:be:ef:00`, IP address 192.168.222.151

Suggestions for automating the functional tests of the DHCP server are appreciated!

