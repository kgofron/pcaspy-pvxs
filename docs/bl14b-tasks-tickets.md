# BL14B HYSPEC — Tasks / tickets for pvxs migration

Task list derived from [kay.md](kay.md) and from BL14B_HYSPEC.csv (SNS BL14B Tasks folder). **Assignment rules:** (1) Rows with **Have PVA?** = **PVXS** are grouped and assigned to **Kaz Gofron**. (2) Rows with no PVXS in **Have PVA?** use **Assign** as primary assignee, with **PPS** and **SE** added when those columns are filled.

---

## Program-level tasks (from Kay steps)

| Ticket | Description | Assignee(s) |
|--------|-------------|-------------|
| T-KAY-1 | **Easy IOCs:** Add pvxs & QSRV dbd/libs to Makefile (add_pvxs.py); test one at a time; coordinate with Barry & Melissa on which IOCs can be updated next | Kaz Gofron (see IOC list below for PVXS group) |
| T-KAY-2 | Move add_pvxs.py to /home/controls/common/??? for reuse at other beam lines | Kaz Gofron (or TBD) |
| T-KAY-3 | **IOCs with older pvAccess/pvData (nED, ADnED):** Run QSRV 2 from PVXS for records; keep custom detector code on old stack until ported to PVXS | Zach (per CSV Assign for Det-ADnED, Det-nED) |
| T-KAY-4 | **Python (pcaspy) IOCs → PyDevice:** Isolate logic from PCAS, bind to records with PyDevice, then add PVXS (~1 day–1 week per IOC) | Per IOC: see Python-pcaspy rows in table below |
| T-KAY-5 | **Custom C++ (Adara, SMS, nED/ADnED):** Rewrite to PVXS; needs someone familiar with modern C++ and PVXS | TBD (C++/PVXS expert) |
| T-KAY-6 | **Older Python PVA clients:** Update scripts (pvaPy / old p4p) to p4p (pvxs); e.g. detector calibration — scope from Greg Guyotte | Greg Guyotte (or TBD) |
| T-KAY-7 | **Permission info:** Track epics-docs PR 140 and PVXS implementation; encourage/fund if needed | TBD |
| T-KAY-8 | **PVA gateway:** Deploy once many IOCs talk PVA; preferably after permission info so gateway can be read-only and GUIs show it | TBD |
| T-KAY-9 | **IOC network link type:** EPICS base “network_link_type=ca\|pva”; encourage/fund if needed | TBD |
| T-KAY-10 | **Secure PVXS:** When EPICS base release includes it (est. 6+ months), update and decide certificate strategy | TBD |

---

## IOC-level tickets (from BL14B_HYSPEC.csv)

### Group: Have PVA? = PVXS → assignee **Kaz Gofron**

These IOCs already have PVXS in the “Have PVA?” column; tasks for validation, rollout, or follow-up are assigned to **Kaz Gofron**.

| IOC | Type | Startup file | Assignee |
|-----|------|--------------|----------|
| bl14b-AlarmHandler | Base | bl14b-AlarmHandler/iocBoot/iocAlarmHandler/st.cmd | **Kaz Gofron** |
| bl14b-ARSupport | Base | bl14b-ARSupport/iocBoot/iocbl14b-ARSupport/st.cmd | **Kaz Gofron** |
| bl14b-Camera1 | Base | bl14b-Camera1/iocBoot/iocbl14b-Camera1/st.cmd | **Kaz Gofron** |
| bl14b-Chop-EnvMon | Base | bl14b-EnvMon/iocBoot/iocbl14b-EnvMon/st.cmd | **Kaz Gofron** |
| bl14b-Chop-ScanAssist1 | Base | bl14b-SkfChopper/iocBoot/.../st-scanAssist-1.cmd | **Kaz Gofron** |
| bl14b-Chop-ScanAssist2 | Base | st-scanAssist-2.cmd | **Kaz Gofron** |
| bl14b-Chop-ScanAssist3 | Base | st-scanAssist-3.cmd | **Kaz Gofron** |
| bl14b-Chop-ScanAssist4 | Base | st-scanAssist-4.cmd | **Kaz Gofron** |
| bl14b-Chop-SKF1 | Base | st-1.cmd | **Kaz Gofron** |
| bl14b-Chop-SKF2 | Base | st-2.cmd | **Kaz Gofron** |
| bl14b-Chop-SKF3 | Base | st-3.cmd | **Kaz Gofron** |
| bl14b-Chop-SKF4 | Base | st-4.cmd | **Kaz Gofron** |
| bl14b-cryostat | Base | bl14b-cryostat/iocBoot/iocbl14b-cryostat/st.cmd | **Kaz Gofron** |
| bl14b-Det_Pwr | Base | bl14b-Det-Pwr/iocBoot/iocbl14b-Det-Pwr/st.cmd | **Kaz Gofron** |
| bl14b-Det-BM | Base | bl14b-Det-BM/iocBoot/iocbl14b-Det-BM/st.cmd | **Kaz Gofron** |
| bl14b-Galil3 | Base | bl14b-Galil3/iocBoot/iocbl14b-Galil3/st.cmd | **Kaz Gofron** |
| bl14b-HighLevel | Base | bl14b-HighLevel/iocBoot/iocbl14b-HighLevel/st.cmd | **Kaz Gofron** |
| bl14b-KeysightWG | Base | bl14b-KeysightWG/iocBoot/iocKeysight33500B/st.cmd | **Kaz Gofron** |
| bl14b-LeakDetector | Base | bl14b-LeakDetector/iocBoot/iocLeakDetection/st.cmd | **Kaz Gofron** |
| bl14b-Mot-Parker2 | Base | bl14b-Parker2/iocBoot/iocbl14b-Parker2/st.cmd | **Kaz Gofron** |
| bl14b-Pol-3DCoils | Base | bl14b-Pol-3DCoils/iocBoot/iocbl14b-Pol-3DCoils/st.cmd | **Kaz Gofron** |
| bl14b-Pol-Agilent | Base | bl14b-Pol-Agilent/iocBoot/iocbl14b-Pol-Agilent/st.cmd | **Kaz Gofron** |
| bl14b-PPS | Base | bl14b-PPS/iocBoot/iocbl14b-PPS/st.cmd | **Kaz Gofron** |
| bl14b-ProcServ-daq1 | Base | bl14b-ProcServ-daq1/iocBoot/ioc/st.cmd | **Kaz Gofron** |
| bl14b-ProcServ-dassrv1 | Base | bl14b-ProcServ-dassrv1/iocBoot/ioc/st.cmd | **Kaz Gofron** |
| bl14b-PS-Spellman | Base | bl14b-PS-Spellman/iocBoot/iocbl14b-PS-Spellman/st.cmd | **Kaz Gofron** |
| bl14b-RunControl | Base | bl14b-RunControl/iocBoot/iocbl14b-RunControl/st.cmd | **Kaz Gofron** |
| bl14b-ScanSupport | Base | bl14b-ScanSupport/iocBoot/bl14b-ScanSupport/st.cmd | **Kaz Gofron** |
| bl14b-SE-GaussMeter | Base | bl14b-SE-GaussMeter/iocBoot/iocGaussMeter/st.cmd | **Kaz Gofron** |
| bl14b-SE-Lakeshore | Base | bl14b-SE-Lakeshore/iocBoot/iocbl14b-SE-Lakeshore/st.cmd | **Kaz Gofron** |
| bl14b-SE-lakeshore460 | Base | bl14b-SE-lakeshore460/iocBoot/iocLakeshore460/st.cmd | **Kaz Gofron** |
| bl14b-SE-Laser | Base | bl14b-SE-Laser/iocBoot/iocFitSam/st.cmd | **Kaz Gofron** |
| bl14b-Status | Base | bl14b-Status/iocBoot/iocbl14b-Status/st.cmd | **Kaz Gofron** |
| bl14b-ThinLincMgmt | PyDevice | bl14b-ThinLincMgmt/iocBoot/iocThinLincMgmt/st.cmd | **Kaz Gofron** |
| bl14b-Timing | Base | bl14b-Timing/iocBoot/iocbl14b-Timing/st.cmd | **Kaz Gofron** |
| bl14b-Vac | Base | bl14b-Vac/iocBoot/iocVac/st.cmd | **Kaz Gofron** |

---

### Group: Type = Base, Have PVA? ≠ PVXS (add PVXS)

Base IOCs that do **not** yet have a PVXS entry in **Have PVA?**. These are candidates for adding pvxs & QSRV dbd/libs to the Makefile (e.g. via add_pvxs.py), coordinated with the Assign and PPS/SE experts. Primary assignee is **Assign**; **PPS** and **SE** are appended when present.

| IOC | Have PVA? | Assignee(s) |
|-----|-----------|--------------|
| bl14b-Cryostat99 | — | Xiaosong Geng; Victor (SE) |
| bl14b-Det-ADnED | — | Zach |
| bl14b-Det-nED | — | Zach |
| bl14b-He3Insert | — | Mariano Ruiz; Victor (SE) |
| bl14b-Mot-Parker1 | — | Xiaosong Geng |
| bl14b-Ophir | — | Mariano Ruiz |
| bl14b-SE-AMI1800 | — | Mariano Ruiz; Victor (SE) |
| bl14b-SE-Dilfridge | — | Mariano Ruiz; Victor (SE) |
| bl14b-SE-dilULT11 | — | Mariano Ruiz; Victor (SE) |
| bl14b-SE-DualValve | — | Xiaosong Geng |
| bl14b-SE-FitSam | — | Mariano Ruiz; Victor (SE) |
| bl14b-SE-Furnace | — | Krishna B; Bekki Mills (SE) |
| bl14b-SE-HeCompressor | — | Mariano Ruiz |
| bl14b-SE-HVPulser | — | Mariano Ruiz |
| bl14b-SE-Keithley2182A | — | Mariano Ruiz |
| bl14b-SE-LHeAF | — | Mariano Ruiz; Victor (SE) |
| bl14b-SE-Mag09DegAF | — | Mariano Ruiz; Victor (SE) |
| bl14b-SE-Nutator | — | Mariano Ruiz |
| bl14b-SE-SlimSam | — | Mariano Ruiz; Victor (SE) |
| bl14b-SE-TeledynePumpDNF | — | Mariano Ruiz; Jamie Molaison (SE) |
| bl14b-SE-TeledynePumpH | — | Mariano Ruiz; Jamie Molaison (SE) |
| bl14b-SE-VacuumCube | — | Mariano Ruiz |
| bl14b-SPFPO | Code changed, not compiled | Mariano Ruiz |
| bl14b-Televac | — | Mariano Ruiz |

---

### Group: Have PVA? ≠ PVXS — PyDevice and Python-pcaspy (and other)

IOCs that do **not** have PVXS in **Have PVA?** and are **not** Type = Base: PyDevice, Python-pcaspy, or other. Primary assignee is **Assign**; **PPS** and **SE** are appended when present.

| IOC | Type | Have PVA? | Assignee(s) |
|-----|------|-----------|--------------|
| bl14b-MonitorPVs | PyDevice | — | Jeeem Kohl |
| bl14b-SE-Triopus | PyDevice | — | Mariano Ruiz |
| bl14b-AdaraMonitor | Python-pcaspy | — | Jeem |
| bl14b-Align | Python-pcaspy | — | Ray Gregory |
| bl14b-AlignXtal | Python-pcaspy | — | Ray Gregory |
| bl14b-AngleCalcs | Python-pcaspy | — | Ray Gregory |
| bl14b-ArchiveEngine | Python-pcaspy | pydevice | Kay Kasemir |
| bl14b-cryostatCurve | Python-pcaspy | — | Mariano Ruiz |
| bl14b-CS-DynamicMapping | Python-pcaspy | — | Ray Gregory |
| bl14b-CursorInfo | Python-pcaspy | — | Ray Gregory |
| bl14b-DGS-CrystalAlign | Python-pcaspy | — | Ray Gregory |
| bl14b-EICManager | Python-pcaspy | — | Ray Gregory |
| bl14b-Energy | Python-pcaspy | — | Kay Kasemir |
| bl14b-Fitting | Python-pcaspy | — | Ray Gregory |
| bl14b-FitXtal | Python-pcaspy | — | Ray Gregory |
| bl14b-He3InsertCurve | Python-pcaspy | — | Mariano Ruiz |
| bl14b-IPTS-ITEMS | Python-pcaspy | pydevice | Kay Kasemir |
| bl14b-PolCalc | Python-pcaspy | — | Mariano Ruiz |
| bl14b-SE-LakeshoreCurve | Python-pcaspy | — | Xiaosong Geng |
| bl14b-SE-StageSwitch | Python-pcaspy | — | Xiaosong Geng |
| bl14b-SE-Switch | Python-pcaspy | — | Xiaosong Geng |
| bl14b-AccessSecurity | — | — | Kay Kasemir |

---

## Summary

- **Kaz Gofron:** All IOCs where Have PVA? = PVXS (37 IOCs), plus program task T-KAY-1 (easy IOCs rollout).
- **Base IOCs not yet PVXS:** 25 Base IOCs with no PVXS in Have PVA? — separate group; add pvxs & QSRV via Makefile; assignees from Assign + PPS + SE.
- **PyDevice / Python-pcaspy (not yet PVXS):** 2 PyDevice + 20 Python-pcaspy + AccessSecurity; assignees from Assign + PPS + SE.
- **Program tasks (T-KAY-*):** Assignees are set from kay.md and CSV where applicable; TBD where no specific expert is named.
- **Source:** BL14B_HYSPEC.csv (SNS BL14B Tasks folder). Update this document if the CSV or assignment rules change.
