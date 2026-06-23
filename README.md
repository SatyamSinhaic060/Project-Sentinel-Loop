# Project Sentinel Loop
1. Project Overview
Project Sentinel Loop is a production-grade, closed-loop SOAR (Security Orchestration, Automation, and
Response) pipeline engineered to orchestrate real-time defensive workflows. By bridging enterprise endpoint
data layers via LimaCharlie with a stateful n8n automation architecture and a locally throttled Google
Gemini AI engine, Project Sentinel Loop mimics a natural immune response to modern endpoint attacks.
Value Proposition: Reduces standard Mean Time to Detect (MTTD) and Mean Time to Respond (MTTR)
down from hours to isolated machine containment in less than 60 seconds.

2. System Architecture & Workflow Loop
The defensive platform executes deterministically via a five-staged architecture loop:
Ingestion: A Slack webhook triggers on high-criticality telemetry alerts sent from a LimaCharlie endpoint
detector.
Forensic Pull: The pipeline automatically contacts LimaCharlie's storage layer to extract raw file artifacts
or compressed packages.
Static AI Engineering: An advanced n8n AI agent strips basic strings and programmatically instructs
gemini-3.5-flash to author a structurally sound, production-ready YARA rule tailored precisely to the
suspicious payload indicators.
Global Deployment: The freshly generated signature is pushed upstream directly into LimaCharlie's
centralized Config Hive structure and instantly set to an active monitoring status.
Autonomous Fleet Containment: A permanent, regex-based master Detection & Response (D&R) rule
matches the signature rollout across the fleet, instructing the local kernel extension to apply network
containment ( history_isolate ) instantly on any positive matches.

# Commands to execute in the console of the LC sensor

run --shell-command "echo X5O!P%@AP[4\PX5O!P)7CC)7}$EICAR-STANDARD-ANTIVIRUS-TEST-FILE!$H+H* > C:\Windows\Temp\eicar_ransomware.bat && echo reg add HKLM\SOFTWARE\Policies\Microsoft\FVE /v EnableBDEWithNoTPM /t REG_DWORD /d 1 /f >> C:\Windows\Temp\eicar_ransomware.bat"

run --shell-command "C:\Windows\Temp\eicar_ransomware.bat"

It triggers the BitLocker detection rule because modifying the registry path HKLM\SOFTWARE\Policies\Microsoft\FVE with the key EnableBDEWithNoTPM is a high-confidence indicator of an active "Living off the Land" Ransomware Attack.

Threat groups—such as ShrinkLocker, Lorenz, and nation-state operators like DEV-0270 (Phosphorus)—frequently abuse Windows' native tools to encrypt systems, allowing them to bypass detection by traditional antivirus engines that flag custom malware binary signatures.

When the D&R rule is triggered the artifacts are collected in LC and slack gets a message with the artifact id and oid.These are used by the http nodes inside the n8n workflow.The code node extracts important strings from the artifact and feeds them to the ai agent that creates YARA rules . These YARA rules are updated in LC with the help of http node. 

# Custom D&R rules 
-Ransomeware
-Dynamic-YARA
