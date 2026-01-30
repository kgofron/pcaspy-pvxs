# SNS Beamline 14B (HYSPEC) — Python IOC migration plan (pvxs)

Plan for enabling pvxs/PVA access for Python IOCs on Spallation Neutron Source (SNS) Beamline 14B (HYSPEC). This document identifies pcaspy and PyDevice IOCs in the BL14B applications tree and ties them to the [plan](plan.md) options.

**Path mappings (production → local copy):**

| Production path | Local copy (this machine) |
|-----------------|---------------------------|
| `/home/controls/bl14b/applications` | `/home/kg1/Documents/SNS/BL14B/bl14b-dassrv1/applications` |
| `/home/controls/common` | `/home/kg1/Documents/SNS/BL14B/bl14b-dassrv1/common` |
| `/opt/mantidworkbench` | `/home/kg1/Documents/SNS/BL14B/bl14b-dassrv1/opt/mantidworkbench` |

---

## 1. pcaspy IOCs (identified)

Python IOCs that use **pcaspy** (Channel Access server in Python). Manual check + scan of `st.cmd` and `*.py` under `applications`:

| IOC name | Startup script (production path) | Startup script (local path) | Notes |
|----------|-----------------------------------|-----------------------------|--------|
| bl14b-AdaraMonitor | `/home/controls/bl14b/applications/bl14b-AdaraMonitor/iocBoot/AdaraMonitor/st.cmd` | `applications/bl14b-AdaraMonitor/iocBoot/AdaraMonitor/st.cmd` | Runs `AdaraMonitorCAS.py` from releases |
| bl14b-Align | `/home/controls/bl14b/applications/bl14b-ScanSupport/iocBoot/Alignment/st.cmd` | `applications/bl14b-ScanSupport/iocBoot/Alignment/st.cmd` | Runs `AlignmentIOC.py` |
| bl14b-AlignXtal | `/home/controls/bl14b/applications/bl14b-ScanSupport/iocBoot/Alignment/st_xtal.cmd` | `applications/bl14b-ScanSupport/iocBoot/Alignment/st_xtal.cmd` | Runs `AlignmentIOC.py` (xtal) |
| bl14b-AngleCalcs | `/home/controls/bl14b/applications/bl14b-AngleCalcs/iocBoot/st.cmd` | `applications/bl14b-AngleCalcs/iocBoot/st.cmd` | Runs `AngleCalcsLaunch.py` |
| bl14b-ArchiveEngine | `/home/controls/bl14b/applications/bl14b-ArchiveEngineStatus/iocBoot/ArchiveEngineStatus/st.cmd` | `applications/bl14b-ArchiveEngineStatus/iocBoot/ArchiveEngineStatus/st.cmd` | Runs `ArchiveEngineStatusIOC.py` from common/archive |
| bl14b-cryostatCurve | `/home/controls/bl14b/applications/bl14b-cryostat/python/st.cmd` | `applications/bl14b-cryostat/python/st.cmd` | Runs LakeshoreCurves.py (common lakeshore) |
| bl14b-CS-DynamicMapping | `/home/controls/bl14b/applications/bl14b-CS-DynamicMapping/iocBoot/st.cmd` | `applications/bl14b-CS-DynamicMapping/iocBoot/st.cmd` | Runs `dynamicMapping.py` |
| bl14b-CursorInfo | `/home/controls/bl14b/applications/bl14b-CursorInfo/st.cmd` | `applications/bl14b-CursorInfo/st.cmd` | Runs `cursor_info.py` from releases |
| bl14b-DGS-CrystalAlign | `/home/controls/bl14b/applications/bl14b-DGS-CrystalAlign/iocBoot/st.cmd` | `applications/bl14b-DGS-CrystalAlign/iocBoot/st.cmd` | Runs `CrystalAlignLaunch.py` |
| bl14b-EICManager | `/home/controls/bl14b/applications/bl14b-EICManager/iocBoot/st.cmd` | `applications/bl14b-EICManager/iocBoot/st.cmd` | Runs `EICManagerLaunch.py` |
| bl14b-Energy | `/home/controls/bl14b/applications/bl14b-Energy/iocBoot/bl14b-Energy/st.cmd` | `applications/bl14b-Energy/iocBoot/bl14b-Energy/st.cmd` | Runs `HyspecEnergy.py` (pcaspy in-tree) |
| bl14b-Fitting | `/home/controls/bl14b/applications/bl14b-ScanSupport/iocBoot/Fitting/st.cmd` | `applications/bl14b-ScanSupport/iocBoot/Fitting/st.cmd` | Runs `FittingIOC.py` |
| bl14b-FitXtal | `/home/controls/bl14b/applications/bl14b-ScanSupport/iocBoot/Fitting/st_xtal.cmd` | `applications/bl14b-ScanSupport/iocBoot/Fitting/st_xtal.cmd` | Runs `FittingIOC.py` (xtal) |
| bl14b-He3InsertCurve | `/home/controls/bl14b/applications/bl14b-He3Insert/python/st.cmd` | `applications/bl14b-He3Insert/python/st.cmd` | Runs LakeshoreCurves.py (common) |
| bl14b-IPTS-ITEMS | `/home/controls/bl14b/applications/bl14b-IPTS/iocBoot/IPTS-ITEMS/st.cmd` | `applications/bl14b-IPTS/iocBoot/IPTS-ITEMS/st.cmd` | Sources common/ipts-items `st_oauth.cmd` |
| bl14b-PolCalc | `/home/controls/bl14b/applications/bl14b-PolCalc/iocBoot/bl14b-PolCalc/st.cmd` | `applications/bl14b-PolCalc/iocBoot/bl14b-PolCalc/st.cmd` | Runs `polCalc.py` (pcaspy in-tree) |
| bl14b-SE-LakeshoreCurve | `/home/controls/bl14b/applications/bl14b-SE-Lakeshore/python/st.cmd` | `applications/bl14b-SE-Lakeshore/python/st.cmd` | Runs LakeshoreCurves.py (common) |
| bl14b-SE-StageSwitch | `/home/controls/bl14b/applications/bl14b-SE-StageSwitch/st.cmd` | `applications/bl14b-SE-StageSwitch/st.cmd` | Runs motor_stage_swap_ioc.py (motion) |
| bl14b-SE-Switch | `/home/controls/bl14b/applications/bl14b-SE-Switch/iocBoot/SE-Switch/st.cmd` | `applications/bl14b-SE-Switch/iocBoot/SE-Switch/st.cmd` | Runs `sampleEnvSelector.py` from releases |

**Count:** 19 pcaspy IOCs (some share ScanSupport boot dirs: Align/AlignXtal, Fitting/FitXtal).

**In-tree pcaspy drivers (import pcaspy in this repo):** Only two application dirs contain Python that imports pcaspy: `bl14b-PolCalc/src/polCalc.py` and `bl14b-Energy/src/HyspecEnergy.py`. The rest use Python scripts from shared locations (releases, common, motion).

---

## 2. PyDevice IOCs (identified)

Python device support in a **C-based soft IOC** (PyDevice). Identified by `pydev.dbd` / `PYDEVICE` in Makefile and configure:

| IOC name | Startup script (local path) | Notes |
|----------|-----------------------------|--------|
| bl14b-MonitorPVs | `applications/bl14b-MonitorPVs/iocBoot/iocMonitorPVs/st.cmd` | C IOC + PyDevice; PYTHONPATH set to MonitorPVs/python |
| bl14b-SE-Triopus | `applications/bl14b-SE-Triopus/iocBoot/iocbl14b-SE-Triopus/st.cmd` | C IOC + PyDevice; has python/pydevtest.py |
| bl14b-ThinLincMgmt | `applications/bl14b-ThinLincMgmt/iocBoot/iocThinLincMgmt/st.cmd` | C IOC + PyDevice; PYTHONPATH to ThinLincMgmt/python |

**Count:** 3 PyDevice IOCs.

**Configure:** Each uses `PYDEVICE` pointing to a PyDevice install (e.g. `/home/controls/common/pydevice/main` on production). On the local copy, `RELEASE.local` or `RELEASE` may need to point to a local PyDevice path.

---

## 3. Sorted migration strategy per IOC

Optimal path for each IOC (short → medium → long term). Sorted by long-term outcome, then by IOC name.

### 3.1 PyDevice IOCs — add QSRV 2 only (no pcaspy migration)

Already C soft IOC + PyDevice; only add pvxs IOC integration so they serve CA + PVA.

| IOC | Short term | Medium term | Long term | Rationale |
|-----|------------|-------------|-----------|-----------|
| bl14b-MonitorPVs | Gateway optional | — | **Add QSRV 2** | C IOC + PyDevice; add dbd/libs for pvxs. |
| bl14b-SE-Triopus | Gateway optional | — | **Add QSRV 2** | C IOC + PyDevice; add dbd/libs for pvxs. |
| bl14b-ThinLincMgmt | Gateway optional | — | **Add QSRV 2** | C IOC + PyDevice; add dbd/libs for pvxs. |

### 3.2 pcaspy IOCs — long-term: PyDevice + QSRV 2 (Option 4)

Heavy Python, shared code, or Mantid; keep logic in Python, move to soft IOC + PyDevice, then add QSRV 2.

| IOC | Short term | Medium term | Long term | Rationale |
|-----|------------|-------------|-----------|-----------|
| bl14b-AngleCalcs | Gateway | p4p alongside (optional) | **PyDevice + QSRV 2** | Mantid; keep Python + conda env. |
| bl14b-ArchiveEngine | Gateway | p4p alongside (optional) | **PyDevice + QSRV 2** | Shared Python (common/archive). |
| bl14b-CS-DynamicMapping | Gateway | p4p alongside (optional) | **PyDevice + QSRV 2** | Mantid; shared dynamicMapping. |
| bl14b-cryostatCurve | Gateway | p4p alongside (optional) | **PyDevice + QSRV 2** | Shared LakeshoreCurves.py. |
| bl14b-DGS-CrystalAlign | Gateway | p4p alongside (optional) | **PyDevice + QSRV 2** | Mantid; CrystalAlignLaunch. |
| bl14b-EICManager | Gateway | p4p alongside (optional) | **PyDevice + QSRV 2** | Shared EICManagerLaunch; access-security glue. |
| bl14b-He3InsertCurve | Gateway | p4p alongside (optional) | **PyDevice + QSRV 2** | Shared LakeshoreCurves.py. |
| bl14b-SE-LakeshoreCurve | Gateway | p4p alongside (optional) | **PyDevice + QSRV 2** | Shared LakeshoreCurves.py. |

### 3.3 pcaspy IOCs — long-term: PyDevice + QSRV 2 (Option 4), scan/shared

ScanSupport or releases-based; Python-centric, so PyDevice is optimal.

| IOC | Short term | Medium term | Long term | Rationale |
|-----|------------|-------------|-----------|-----------|
| bl14b-AdaraMonitor | Gateway | p4p alongside (optional) | **PyDevice + QSRV 2** | Shared DAS/AdaraMonitorCAS. |
| bl14b-Align | Gateway | p4p alongside (optional) | **PyDevice + QSRV 2** | ScanSupport; AlignmentIOC. |
| bl14b-AlignXtal | Gateway | p4p alongside (optional) | **PyDevice + QSRV 2** | Same as Align (xtal boot). |
| bl14b-CursorInfo | Gateway | p4p alongside (optional) | **PyDevice + QSRV 2** | Releases cursor_info. |
| bl14b-Fitting | Gateway | p4p alongside (optional) | **PyDevice + QSRV 2** | ScanSupport; FittingIOC. |
| bl14b-FitXtal | Gateway | p4p alongside (optional) | **PyDevice + QSRV 2** | Same as Fitting (xtal boot). |
| bl14b-IPTS-ITEMS | Gateway | p4p alongside (optional) | **PyDevice + QSRV 2** | Common ipts-items; OAuth/items logic. |
| bl14b-SE-Switch | Gateway | p4p alongside (optional) | **PyDevice + QSRV 2** | Releases sampleEnvSelector. |

### 3.4 pcaspy IOCs — long-term: PyDevice or C++ (Option 4 or 3)

In-tree pcaspy or simple glue; PyDevice preferred to keep Python, C++ only if team wants no Python in IOC.

| IOC | Short term | Medium term | Long term | Rationale |
|-----|------------|-------------|-----------|-----------|
| bl14b-Energy | Gateway | **p4p alongside** (prototype) | **PyDevice + QSRV 2** (preferred) or C++ | In-tree HyspecEnergy.py; PyDevice reuses driver. |
| bl14b-PolCalc | Gateway | **p4p alongside** (prototype) | **PyDevice + QSRV 2** (preferred) or C++ | In-tree polCalc.py; PyDevice reuses driver. |
| bl14b-SE-StageSwitch | Gateway | p4p alongside (optional) | **PyDevice + QSRV 2** or C++ | Motion script; small glue → C++ possible. |

**Summary:** All 19 pcaspy IOCs: short term = Gateway; medium term = p4p alongside for Energy/PolCalc first, then others as needed; long term = **PyDevice + QSRV 2** for all except optionally C++ for bl14b-Energy, bl14b-PolCalc, bl14b-SE-StageSwitch if the team prefers no Python in those IOCs. The three Mantid IOCs and all shared-Python IOCs should use PyDevice.

---

## 4. Migration options applied to BL14B

From [plan.md](plan.md):

- **Short term:** Run a CA-to-PVA gateway (Option 2) in front of the 19 pcaspy IOCs (and optionally in front of the 3 PyDevice IOCs if PVA is desired for them). No changes to existing IOC code.
- **Medium term:** Add p4p PVA server alongside selected pcaspy IOCs (Option 1). Start with IOCs that have in-tree drivers (bl14b-PolCalc, bl14b-Energy) for easier mirroring; then extend to launch scripts that wrap shared pcaspy code.
- **Long term:** Migrate pcaspy IOCs to either C++ soft IOC + QSRV 2 (Option 3) or PyDevice + QSRV 2 (Option 4). PyDevice IOCs already use standard EPICS soft IOC; add QSRV 2 (pvxs) to those three to expose the same database over PVA.

---

## 5. Suggested next steps for BL14B

1. **Inventory shared Python:** Identify where the pcaspy drivers live for IOCs that run from releases/common (AdaraMonitor, CursorInfo, SE-Switch, ArchiveEngine, IPTS-ITEMS, Lakeshore curves, etc.). This determines what must be mirrored or migrated for Option 1 or 3/4.
2. **Path mapping for local dev:** In the local copy, adjust `st.cmd` / envPaths to use local base paths (e.g. `$(TOP)` or `/home/kg1/Documents/SNS/BL14B/bl14b-dassrv1/...`) where scripts and common modules are copied.
3. **Prototype Option 1 on one IOC:** Add p4p alongside bl14b-Energy or bl14b-PolCalc (in-tree pcaspy); validate PVA client access.
4. **Enable QSRV 2 on PyDevice IOCs:** Add pvxs IOC integration (dbd, libs) to bl14b-MonitorPVs, bl14b-SE-Triopus, bl14b-ThinLincMgmt so they serve CA + PVA without a gateway.
5. **Prioritise pcaspy IOCs for long-term migration:** Use the sorted strategy in §3; **Mantid-using IOCs** (bl14b-AngleCalcs, bl14b-CS-DynamicMapping, bl14b-DGS-CrystalAlign) and all shared-Python IOCs → PyDevice (Option 4); see §7.

---

## 6. Reference: local applications root

All paths in this document are relative to:

**Local applications root:** `/home/kg1/Documents/SNS/BL14B/bl14b-dassrv1/applications`

So e.g. `applications/bl14b-Energy/iocBoot/bl14b-Energy/st.cmd` means  
`/home/kg1/Documents/SNS/BL14B/bl14b-dassrv1/applications/bl14b-Energy/iocBoot/bl14b-Energy/st.cmd`.

---

## 7. Mantid-using IOCs and long-term migration strategy

### 7.1 Which IOCs use Mantid

Identified by `conda activate py310_mantid` and/or `--mantid-base-path "/opt/mantidworkbench"` in startup scripts under `applications`:

| IOC name | Startup script (local) | Mantid usage |
|----------|------------------------|--------------|
| **bl14b-AngleCalcs** | `applications/bl14b-AngleCalcs/iocBoot/st.cmd` | `conda activate py310_mantid`; `--mantid-base-path "/opt/mantidworkbench"` passed to `AngleCalcsLaunch.py` (Python from opi/AngleCalcs/../python/) |
| **bl14b-CS-DynamicMapping** | `applications/bl14b-CS-DynamicMapping/iocBoot/st.cmd` | `conda activate py310_mantid`; runs `dynamicMapping.py` from common/dynamic-mapping |
| **bl14b-DGS-CrystalAlign** | `applications/bl14b-DGS-CrystalAlign/iocBoot/st.cmd` | `conda activate py310_mantid`; `--mantid-base-path "/opt/mantidworkbench"` passed to `CrystalAlignLaunch.py` (Python from opi/DGS-CrystalAlignment/../python/) |

**Count:** 3 pcaspy IOCs use a Mantid-enabled environment. The Python that imports Mantid lives in shared locations (bl14b/opi/.../python, common/dynamic-mapping), not under `applications`.

**Mantid path on BL14B:**

- **Production (bl14b-dassrv1):** `/opt/mantidworkbench`
- **Local copy (this machine):** `/home/kg1/Documents/SNS/BL14B/bl14b-dassrv1/opt/mantidworkbench`

---

### 7.2 Long-term strategy: C++ vs PyDevice for Mantid IOCs

**C++ (Option 3 — rewrite as C++ soft IOC + QSRV 2):**

- Mantid is a large Python/C++ framework for neutron data reduction; algorithm workflows are typically driven from Python. There is no drop-in way to "move" this logic into a C++ IOC.
- Options would be: (a) reimplement the same science/calculations in C++ (huge effort, duplicate of Mantid), or (b) keep a separate Mantid Python process and have the C++ IOC talk to it via RPC/sockets (adds process and interface complexity, still depends on Python + Mantid elsewhere).
- Using Mantid's C++ API from inside the IOC is possible in theory but would require building the IOC against Mantid, matching the algorithm set and versions, and maintaining that stack. High cost and coupling.

**PyDevice (Option 4 — migrate to C soft IOC + PyDevice + QSRV 2):**

- The IOC becomes a standard EPICS soft IOC; **Python runs only as device support** (PyDevice). The same **Mantid-enabled conda env** (e.g. `py310_mantid`) and the same **Python code** (AngleCalcsLaunch, CrystalAlignLaunch, dynamicMapping logic) can be reused by having PyDevice records call into modules that import Mantid and perform the calculations.
- No need to reimplement Mantid logic in C++; no extra "helper" process. You keep one process (soft IOC + embedded Python), add QSRV 2 for native CA + PVA (pvxs), and continue using Mantid from Python inside that process.

**Conclusion:** For the three Mantid-using IOCs (**bl14b-AngleCalcs**, **bl14b-CS-DynamicMapping**, **bl14b-DGS-CrystalAlign**), **PyDevice (Option 4) is the better long-term migration path** than C++ (Option 3). Use PyDevice + QSRV 2 so these IOCs keep Python and Mantid, gain pvxs, and avoid rewriting Mantid-dependent logic in C++.
