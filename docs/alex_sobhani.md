# pcaspy_wrapper

```
Here it is:

https://code.ornl.gov/idac/ex/pcaspy_wrapper/-/blob/main/driver.py?ref_type=heads

Very little code, but this got the same behavior for the IPTS ITEMS IOC as regular pcaspy as far as I could tell. The current version of the wrapper hosts both the CA and PVA PVs. To set up, the only thing that needs to be done on the IOC side is replace "from pcaspy import ..." with "from pcaspy_wrapper import ..."

I did not add support for things like arrays, and limits, so I would have to add support for those.

Also, I started it in pvapy because that is what I am familiar with, with the intention of adding support for p4p later (I think this would probably not be a lot of work since the libraries are similar).

I think it may make sense to wait a little bit and see if this works before diving in and rewriting all the various one-off pcaspy IOCs.

I think development of this should be quick but the main thing that would delay me is setting up IOCs to test with. I was going to set up the alignment scan IOC with dummy PVs in bl100 to test with, because I believe that makes use of array PVs. Then maybe after I am done with this we can try testing it with various HYSPEC IOCs.
```
