# Evaluation: pcaspy_wrapper (incorporating PVA into pcaspy without rewriting IOCs)

This document evaluates the **pcaspy_wrapper** approach ([alex_sobhani.md](alex_sobhani.md), repo: `/home/kg1/Documents/src/gitlab/pcaspy_wrapper`) for giving existing pcaspy IOCs PVA/pvxs access with minimal change, and sets it in context of the pcaspy author’s view and the broader migration plan.

---

## 1. What pcaspy_wrapper does

- **Idea:** Provide a drop-in replacement for pcaspy that serves **both CA and PVA** in the same process. The only IOC-side change is to replace `from pcaspy import ...` with `from pcaspy_wrapper import ...`.
- **Mechanism:** Subclass pcaspy’s `SimplePV`, `Driver`, and `SimpleServer`; for each CA PV, also create a PVA record (e.g. `pvapy.PvFloat()`) and register it with a PVA server. A callback on the PVA side calls back into the driver’s `write()` so CA and PVA stay in sync.
- **Current implementation:** Uses **pvapy** (classic pvDataCPP/pvAccessCPP stack), not p4p/pvxs. Alex’s note: adding p4p support later is intended and “probably not a lot of work since the libraries are similar.”
- **Gaps (Alex):** No support yet for arrays and limits; would need to be added. Testing was with IPTS ITEMS IOC; alignment scan IOC (array PVs) was planned for bl100.

**Conclusion (technical):** pcaspy_wrapper is a viable “add PVA to pcaspy IOCs without rewriting them”: one process, two protocols, minimal code change. To align with the **pvxs stack** (target for BL14B/SNS), the wrapper should be ported from pvapy to **p4p** so that the PVA side is pvxs-based.

---

## 2. View of Wang, Xiaoqiang (pcaspy author)

From [pcaspy.md](pcaspy.md):

- Xiaoqiang considered adding PVA to pcaspy “several times” but **did not see the benefits**, because **Michael Davidsaver already provides the Python binding** for PVA — i.e. **p4p** ([epics-base.github.io/p4p/server.html](https://epics-base.github.io/p4p/server.html)).
- He clarifies that **PVA is the protocol name**; there are two C++ implementations, each with its own Python binding: (1) pvDataCPP / normativeTypesCPP / pvAccessCPP → **pvaPy**, (2) **pvxs** → **p4p**.

**Implication for pcaspy_wrapper:** The pcaspy author’s position is that the “right” way to do PVA in Python is to use p4p (or pvaPy), not to extend pcaspy itself. pcaspy_wrapper is a **third path**: keep the pcaspy programming model and IOC code, but add a PVA face via a wrapper. That is consistent with using p4p (or pvapy) under the hood; it does not contradict Xiaoqiang’s view as long as the wrapper is seen as “using the existing PVA Python binding” (p4p/pvapy) to serve the same PVs that pcaspy serves over CA. The open choice is whether to standardise on **p4p (pvxs)** in the wrapper for SNS/BL14B.

---

## 3. Evaluation summary

| Criterion | Assessment |
|-----------|------------|
| **Minimal IOC change** | Yes: change imports only; no rewrite of driver logic. |
| **pvxs alignment** | Not yet: current code uses pvapy (classic). Porting the PVA side to **p4p** would align with pvxs stack and [plan](plan.md). |
| **Scope** | Scalars work (per Alex); arrays and limits need to be added. Suited to “get PVA on the wire” for many simple pcaspy IOCs while longer-term options (PyDevice, C++) are pursued. |
| **Risk** | Low if done per-IOC with testing; wrapper is small and maintainable. Dependency on pcaspy’s internal behaviour (e.g. `SimplePV` / driver callback API). |
| **Fit with plan** | Sits between **Option 1** (p4p alongside pcaspy, hand-written mirroring) and **Option 2** (external gateway). Option 1b: **pcaspy_wrapper** — same idea as Option 1 but packaged as a drop-in layer; once ported to p4p, delivers pvxs in process without rewriting IOCs. |

**Recommendation:** Treat pcaspy_wrapper as a valid **short- to medium-term** path to get PVA (and, after p4p port, **pvxs**) on existing pcaspy IOCs with minimal change. Long-term migration (e.g. PyDevice, C++, or p4p-only) can proceed in parallel; the wrapper does not block it and can reduce pressure to rewrite IOCs immediately.

---

## 4. Python IOCs are only one part of migration

Migration to the pvxs stack is broader than Python IOCs:

- **C / C++ Base IOCs:** Add pvxs & QSRV 2 to the Makefile (dbd, libs), then reboot (“easy” IOCs).
- **IOCs already using older pvAccess/pvData libs:** e.g. nED, ADnED — run QSRV 2 for records while leaving custom PVA code on the old stack until it is ported to PVXS.
- **Python (pcaspy) IOCs:** Options include pcaspy_wrapper (with p4p), p4p alongside, gateway, or migration to PyDevice.
- **Custom C++ (Adara, SMS, detector custom code):** Must be rewritten to use PVXS.
- **Python PVA clients:** Update from pvaPy or old p4p to **p4p (pvxs)**; coordinate with script owners (e.g. Greg Guyotte for calibration scripts).
- **Infrastructure:** PVA gateway, permission info in protocol, IOC network link type (CA vs PVA), secure PVXS when released.

The pcaspy_wrapper addresses only the “existing pcaspy IOCs” slice; the rest of the plan (e.g. [plan.md](plan.md), [kay.md](kay.md)) still applies.
