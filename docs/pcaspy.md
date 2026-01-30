# pcaspy pvxs

* Wang, Xiaoqiang
  * xiaoqiang.wang@psi.ch



```
Hi Kaz,

I thought about it several times, but couldn’t see the benefits because Michael Davidsaver provides the Python binding,
https://urldefense.us/v2/url?u=https-3A__epics-2Dbase.github.io_p4p_server.html&d=DwIGaQ&c=v4IIwRuZAmwupIjowmMWUmLasxPEgYsgNI-O7C4ViYc&r=EfANTzS5jEGdEZzE7yeVMfa-1AMzSjBUhFmt9OWyhj8&m=l46qF5pWtSwG5n0RryaOoJMi7rPz4KmtCPYlbZW3rXA5Nerl8J3m1u9VIcWLJ16-&s=nzDpTMtDSOWzEHnGqrfuvH9L81-f-YYO4KSmywmEW2A&e=
```

* Is pva same as pvxs
```
Hi Kaz,

This is my understanding: PVA is the protocol name.
There are two C++ implementations in epics-base and each has its own python binding.
1. pvDataCPP/normativeTypesCPP/pvAccessCPP and pvaPy
2. pvxs and p4p

Best
Xiaoqiang
```

### PVA is a protocol name

### 
* https://github.com/epics-base/p4p
* https://github.com/epics-base/pvxs

### pvaPy
* https://github.com/epics-base/pvDataCPP
* https://github.com/epics-base/normativeTypesCPP
* https://github.com/epics-base/pvAccessCPP
* https://github.com/epics-base/pvaPy

---

## Migrating pcaspy IOCs to pvxs (PyDevice)

For migrating pcaspy IOCs to pvxs via **PyDevice** (C soft IOC + Python device support + QSRV 2), see **Option 4** in [plan.md](plan.md). For BL14B-specific strategy (which IOCs use PyDevice, Mantid, sorted migration per IOC), see [bl14b-migration-plan.md](bl14b-migration-plan.md).

