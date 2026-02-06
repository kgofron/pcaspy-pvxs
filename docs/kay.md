# Kay

* PVXS is mentioned on the 7.1 roadmap, 
  * https://github.com/epics-base/epics-base/wiki/Road%E2%80%90map-for-EPICS-7.1,

'''
As for “Secure EPICS project” in this email chain title, the ongoing security work remains on a PVXS branch, see attached copy of earlier emails. As for when it might be merged, all I can offer is a vague feeling “late 2026”. Also note in the earlier email chain we talked about “PVA at an SNS beam line”, not “Secure EPICS”. The suggestion was to go with the released PVXS to get a foot into the door towards PVA, and be prepared for “secure” once that’s merged.
```

### Steps

* Here’s an idea for a list of steps or tasks that could be tracked in some system of your choice.

```
Update all the ‘easy’ IOCs
For most of the 60 IOCs, we simply add the pvxs & qsrv dbd and libs to the IOC Makefile, reboot, done.
They do not use the older pvAccess/pvData/pv* libs, so there’s no conflict between older and newer PVA libs.
In most cases, the ~ky9/BL14B_PVA_Update/add_pvxs.py script locates the Makefile and performs the update.
At some point we might simply do that in bulk, but for a while it’s a better idea to do this one at a time and test each one if it still works.
.. which means we need to wait until SE equipment is available etc., need to somehow coordinate with Barry & Melissa which IOCs can be updated next.
The script might need tweaks to handle some IOCs with more elaborate Makefiles.
At some point, we can move that script to /home/controls/common/??? for later use at other beam lines.

Update the IOCs that already link the older pvAccess, pvData, pv* libs
These are nED and ADnED IOCs, maybe a few more.
I believe they don’t want to serve records, they run custom code to serve ’neutrons’ or other custom detector PVs.
It is​ possible to have an IOC with custom code based on the older pvAccess, .. libs, but NOT running the older qsrv, and instead run the newer qsrv2 from PVXS for all the records. That would get us most of the way, all the records via PVXS, and the ’neutrons’ still work. In the fullness of time, the custom code would be updated to PVXS.

Update python CA servers (python IOCs) to pyDevice (times ~20)
There are about 20 of them, and each one will become its own project/task, taking between a day and a week of work.
Isolate the logic from the PCAS binding, then use pyDevice to bind the logic to records.
Then treat like an ‘easy’ IOC to add PVXS.

Update custom C++ code (Adara, SMS, what’s left in nED, ADnED IOCs)
This means the Adara services that were created with the older pvAccess libs.
Also includes the custom code in IOCs that already link the older pvAccess, … which might have been left in place in the step above.
Needs to be rewritten with PVXS. Needs somebody familiar with newer C++ features.

Update older python PVA clients
I believe there are some scripts implemented in python and the older pvaPy used to calibrate detectors etc. Maybe Greg Guyotte knows more?
Should be updated to the latest p4p, which is based on PVXS.
If they used the older p4p, based on the older pvAccess lib, the update may be trivial, no change to python, just run with newer p4p. 

Permission info
With CA, the client knows if we have write access. The GUI can show that you can’t write (widget is ‘ghosted’, STOP sign for cursor, …).
Right now, PVA clients don’t know if they have write access, it’s not supported by the protocol
This is https://github.com/epics-docs/epics-docs/pull/140, a PR to update the PVA protocol to communicate write permissions.
It has been discussed with Michael and already been implemented in Java, but we don’t have the final “yes, that’s really it” nor the PVXS implementation.
 Maybe we can encourage/fund it, but out of our hands. 

PVA Gateway
Once a “considerable” number of IOCs talk PVA, we can add a PVA gateway to see those from the office network.
Would make more sense to do that after the permissions info is available, because the gateway will start out read-only just like the CA gateways, and we should see in the GUI that it’s read-only.

IOC network links
field(INP, “ABC”) will automatically become a database link when record(xx, “ABC”) is in the same IOC, or a network = Channel Access link when there is no “ABC” record in the same IOC.
That’s a great feature of EPICS, allowing you to move calc records and the like around.
.. but all network links are automatically using Channel Access.
You can force a PVA network link like this:

    field(INP, {pva:{
        pv:”ABC",
        field:"",
        local:false,
        Q:4,
        pipeline:false,
        proc:none,
        sevr:false,
        time:false,
        monorder:0,
        retry:false,
        always:false,
        defer:false
    }})

That clearly sucks, plus if you do that, you always get a PVA link. You lose the automatic behavior of getting a database vs. network link.
—> IOC needs a global switch “network_link_type=ca|pva”. That’s something that needs to happen in EPICS base. Maybe we can encourage/fund it, but out of our hands. 

Update to the latest “secure” PVXS
The secure PVXS development is still on a branch. CSS has been tracking it, so the current CSS is already compatible, but we need to wait at least 6 months, likely longer, until an EPICS base release will include secure PVXS.
When that time comes, we want to update to it and decide on one of the options for creating certificates.
```
