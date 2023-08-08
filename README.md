# opennebula.tm.ssh

This TM driver for OpenNebula is slightly ajusted to modify the replica host to the destination host (every host is a replica host). This allows fast VM startups in environments with just a few hosts and a limited number of images. The echo outputs could be removed to not let the logfiles grow indefinitely, but they are quite helpful to understand what's going on.

Replace the files in `/var/lib/one/remotes/tm/custom_ssh/` with the files from this repository.

```
scp ./* onefrontend:/var/lib/one/remotes/tm/custom_ssh/
ssh onefrontend
scp /var/lib/one/remotes/tm/custom_ssh/* compute01:/var/tmp/one/tm/custom_ssh/
scp /var/lib/one/remotes/tm/custom_ssh/* compute02:/var/tmp/one/tm/custom_ssh/
...
```

I also updated the scheduler setttings to allow scheduling four VMs at once per host. In my case, this allows spawning 10 VMs per host in around two minutes (boot inclusive, with an already replicated image). Before modifying the driver only one host was the replcia host and a 17 GiB large image was transferred once per VM to the other compute nodes taking more than 2 minutes per transfer on a gigabit link.
