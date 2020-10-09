# 10 Problems I Learned to Solve With Ansible

## 1. Use a dynamic inventory
Static inventory files are easy to write, but dynamic inventories are your friend!  I was a little putoff of them at first.  I wanted to be able to build custom groups and I had no idea what the inventory file would actually look like if I didn't have it defined.  Most cloud providers and on-prem hypervisors have support for adding tags to created machines.  These tags create groups in the inventory and populate any tagged machine into that group.
The second problem is nicely handled with the ansible-inventory command.  You could even write the output of this command into a static file if you wanted to commit a static inventory to version control.
```ansible-inventory -i dynamicinventory.yml --list```

I started my Ansible life writing static inventory files.  If I used variables in the inventory, I was using them to define connection attributes such as a different username, SSH port, or WinRM parameter.  Then I learned how to use a dynamic inventory/inventory plugin.  This hugely upped my game because now I could build a machine and connect to it without having to set a static IP address.  I could query 

## 2. I should have been using the inventory directory much more thoroughly
Inventory directories are hugely important.  They deal with the multi-cloud issue nicely because you can have two dynamic inventory files in there that could, for example, query VMWare and AWS and group machines from each environment in the same group.  Perhaps you want to update all your RHEL 7 machines in both environments.  This will let you smoothly target them.  It's also extremely handy when I need to dynamically extend my network into the cloud.  I can provision each endpoint and obtain information about the remote endpoint on the fly.

## 3. Inventory group_vars and host_vars
I frequently need to deconstruct complex data structures in Ansible and rebuild them.  Sometimes this is a matter of me traversing a list for everything that matches an attribute and then making a list of a different attribute to be consumed.  I might want to select every machine that has RHEL7 installed and make a list of those IP addresses.  I used to put this sort of data in a role because that's where it seemed to belong, but it's WAY easier in an inventory file because now I can quickly test my filters with 

## 4. Complex data structures will natively output as JSON using the copy module content attribute.
If something can take in templates, I no longer have to build a template file and add logic to support non-existent parameters or extend my logic to support new parameters in something like Packer.

## 5. Loop control!  
You can change things from being "item", truncate annoying loop output with a label, etc.

## 6. YAML Anchors
Sometimes you have a dictionary where you 