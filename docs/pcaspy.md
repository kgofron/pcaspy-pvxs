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

## Option 4: Migrate to PyDevice (C soft IOC + Python device support) + QSRV 2

**PyDevice** ([klemenv/PyDevice](https://github.com/klemenv/PyDevice)) is EPICS device support that embeds a Python interpreter inside a **C-based soft IOC**. Records use `DTYP="pydev"` and INP/OUT links like `@device1.connect()` or `@device1.sent`; record processing runs Python code (expressions for inputs, statements for outputs). The IOC itself is a standard EPICS Base soft IOC (C), so it already serves CA; adding **QSRV 2** (pvxs) to that IOC gives dual CA + PVA (pvxs) with no extra gateway.

**Migration path:** Rewrite the pcaspy IOC as a PyDevice IOC: (1) Define an EPICS database (`.db`) with the same PVs, using record types supported by PyDevice (longin, longout, ai, ao, bi, bo, stringin, stringout, waveform, etc.) and `DTYP="pydev"`. (2) Move pcaspy Driver logic into Python modules (classes/functions) that PyDevice calls from INP/OUT (e.g. `@mymodule.device.read()`). (3) Build the IOC with PyDevice and EPICS Base (and pvxs if desired). (4) Add QSRV 2 (pvxs IOC integration) to the same IOC so it serves the database over PVA.

**Pros:** Standard EPICS architecture (soft IOC + device support); Python keeps device/application logic; single process serves both CA and PVA (pvxs) once QSRV 2 is enabled; no mirroring or gateway; PyDevice supports I/O Intr scanning via `pydev.iointr()` for push-from-Python updates.

**Cons:** Model change from pcaspy (Driver owns PVs, server-centric) to record-centric (each record triggers Python); database and link syntax (`@...`) must be written/maintained; PyDevice supports a fixed set of record types; migration effort to map existing pcaspy PVs and Driver methods to records + Python calls.

**Feasibility:** High. PyDevice is an existing, maintained project (GPL-3.0); build requires EPICS Base and Python dev headers. pvxs/QSRV 2 integration is standard for any EPICS Base IOC (add dbd, libs, and env as per [pvxs IOC integration](https://epics-base.github.io/pvxs/ioc.html)).

