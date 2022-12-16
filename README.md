# opennebula.tm.ssh

This TM driver for OpenNebula is a slightly ajusted SSH driver to modify the replica host to the destination host (so every host is a replica host). This allows fast VM startups for environments with just a few hosts and a limited number of images. The echo outputs could be removed to not let the logfiles grow indefinitely, but they are quite helpful to understand what's going on.

Replace the files in `/var/lib/one/remotes/tm/ssh/` with the files from this repository. If you want the log messages to see whats going on you can also transfer the modified driver to your compute nodes which is going to create a log their as well, letting you see which operations are invoked.

```
scp ./* onefrontend:/var/lib/one/remotes/tm/ssh/
ssh onefrontend
scp /var/lib/one/remotes/tm/ssh/* compute01:/var/tmp/one/tm/ssh/
scp /var/lib/one/remotes/tm/ssh/* compute02:/var/tmp/one/tm/ssh/
...
```

I also updated the scheduler to allow scheduling four VMs at once per host. In my case this allows spawning 10 VMs per host in around two minutes (boot inclusive, image was already replicated). Before modifying the driver only one host was the replcia host and the 17 GiB large image was transferred once per VM to the other compute nodes taking more than 2 minutes per transfer on a standard gigabit link.
