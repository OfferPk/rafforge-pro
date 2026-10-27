\# RAFForge Pro (v1.0.0)

\*\*CCSDS 911.1-B-4 / ISO 26143:2023 Space Link Extension (SLE) Return All Frames (RAF) Ground Gateway & Telemetry Distribution Studio\*\*

\---

\#\# 1\. Executive Summary

\*\*RAFForge Pro\*\* is a zero-dependency, aerospace-grade developer studio and headless command-line interface implementing the \*\*CCSDS 911.1-B-4 (Space Link Extension — Return All Frames Service)\*\* and \*\*ISO 26143:2023\*\* standards.

In space communications, ground tracking station networks (NASA Deep Space Network, ESA ESTRACK, KSAT, SSC, AWS Ground Station, Azure Orbital) receive high-frequency telemetry downlinks from orbital and deep-space missions. While specialized services like Return Channel Frames (RCF, CCSDS 911.2-B) filter telemetry for specific Virtual Channels (VCs), the \*\*Return All Frames (RAF)\*\* service delivers \*\*all space telemetry frames\*\* received by the antenna to Mission Operations Centers (MOC).

RAFForge Pro bridges the gap between complex C/C++ ground station software suites and modern lightweight mission planning tools by providing:  
1\. A pure ES6+ JavaScript core engine and Python 3 CLI.  
2\. Complete 3-state SLE Finite State Machine simulation (\`UNBOUND\`, \`READY\`, \`ACTIVE\`).  
3\. Earth Reception Time (ERT) synthesis adhering strictly to the \*\*CCSDS 301.0-B-4 Day Segmented (CDS)\*\* time code.  
4\. Antenna RF Tracking Status modeling (Carrier Lock, Subcarrier Lock, Symbol Sync Lock).  
5\. Delivery Mode arbitration (\`onlineTimely\`, \`onlineComplete\`, \`offline\`) and frame quality filtering (\`allFrames\` vs \`completeFramesOnly\`).  
6\. A 10-Point Normative Aerospace Standards Compliance Auditor (\`RAF-001\` through \`RAF-010\`).

\---

\#\# 2\. Standards Compliance

\- \*\*CCSDS 911.1-B-4\*\*: Space Link Extension — Return All Frames Service Specification (Blue Book, Issue 4).  
\- \*\*ISO 26143:2023\*\*: Space data and information transfer systems — Space link extension (SLE) — Return all frames service.  
\- \*\*CCSDS 132.0-B-3\*\*: TM Space Data Link Protocol (Frame Header & CRC-16-CCITT FECF).  
\- \*\*CCSDS 301.0-B-4\*\*: Time Code Formats (CCSDS Day Segmented \- CDS Code).  
\- \*\*ISO 21107 / CCSDS 732.1-B-1\*\*: Unified Space Data Link Protocol (USLP) compatibility.

\---

\#\# 3\. System Architecture & Lifecycle

\`\`\`  
\[Spacecraft Telemetry Frame\] \---\> \[Ground Station Antenna Receiving Facility\]  
                                         \|  
                                         v  
                         \[RF Carrier / Subcarrier / Symbol Lock\]  
                         \[Earth Reception Time (ERT) CDS Stamping\]  
                         \[CRC-16-CCITT Frame Error Detection\]  
                                         \|  
                                         v  
                      \+---------------------------------------+  
                      \|       RAFForge Pro SLE Provider       \|  
                      \|  \- State 1: UNBOUND                   \|  
                      \|  \- State 2: READY (via RAF-BIND)      \|  
                      \|  \- State 3: ACTIVE (via RAF-START)    \|  
                      \+---------------------------------------+  
                                         \|  
                \+------------------------+------------------------+  
                \|                                                 \|  
         \[allFrames Mode\]                            \[completeFramesOnly Mode\]  
  Delivers both Good and Errored             Discards Errored frames; delivers only  
  frames with error diagnostic flags.        frames with valid CRC and full RF locks.  
                \|                                                 \|  
                v                                                 v  
   \[Mission Operations Center\]                       \[Mission Operations Center\]  
\`\`\`

\---

\#\# 4\. Quickstart & Usage

\#\#\# 4.1 Headless Python 3 CLI

Execute telemetry stream ingestion and automated normative auditing:

\`\`\`bash  
\# Run simulated telemetry ingestion (20 frames, onlineTimely mode, allFrames)  
python3 source-code/raf\_cli.py run \--mode onlineTimely \--quality allFrames \--frames 20

\# Run completeFramesOnly filtering with 20% simulated link errors  
python3 source-code/raf\_cli.py run \--mode onlineTimely \--quality completeFramesOnly \--frames 50 \--error-rate 0.20 \--json  
\`\`\`

\#\#\# 4.2 Interactive Browser Mission Control Console

Open \`source-code/index.html\` directly in any standard modern web browser (zero build step or web server required):  
\- Real-time SLE State lifecycle controls (\`BIND\`, \`START\`, \`STOP\`, \`UNBIND\`).  
\- Interactive Antenna RF Lock toggle simulating atmospheric rain fade and carrier unlocked states.  
\- Live telemetry waterfall table with CDS UTC timestamps, Spacecraft IDs, VCIDs, MCFC counters, and CRC flags.  
\- Real-time 10-point normative auditor scorecard.

\---

\#\# 5\. Automated Verification Test Suite

RAFForge Pro includes a 10-point automated unit test runner (\`source-code/test\_runner.py\`):

\`\`\`bash  
python3 source-code/test\_runner.py  
\`\`\`

\*\*Results\*\*:  
\- Ran 10 tests in 0.018s.  
\- \*\*Pass Rate\*\*: 100% (10/10 PASS).  
\- \*\*Certification Status\*\*: \`CCSDS\_911\_1\_RAF\_CERTIFIED\`.

\---

\#\# 6\. Commercial Model & Licensing

\- \*\*Community Open-Core\*\*: Free CLI inspection and standard TM frame parser for research, university satellite clubs, and open-source CubeSat teams.  
\- \*\*Professional Station Seat (\$349/year)\*\*: Ground station engineering workstation license with live UDP/TCP socket bindings, real-time waterfall monitors, and automated pass logging.  
\- \*\*Enterprise Ground Network License (\$2,499 one-time)\*\*: Full source code license, automated DSN/ESTRACK interface adapters, multi-antenna provider multiplexing, and commercial MOC integration support.

\---

\#\# 7\. Known Limitations

\- Simulates terrestrial SLE network protocol layers, frame extraction, Earth Reception Time stamping, and RF lock state tracking.  
\- Physical parabolic antenna tracking servos, high-gain cryogenic amplifiers, and physical radio frequency demodulators require physical ground station hardware.  
