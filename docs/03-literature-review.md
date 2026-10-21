# Literature Review

## 1. Purpose and scope

The review supports the design decisions of WorkTrack and answers four questions:

1. How accurate is smartphone positioning, especially indoors, and what does this mean for geofence decisions?
2. How can location data be falsified, and what can an application realistically detect?
3. What is known about attendance systems based on geofences, face recognition and QR codes?
4. What do studies and regulators say about monitoring employees, trust and personal data?

**Source requirements (committee):** at least 12 meaningfully used sources, at least 8 scientific publications;
for fast-changing topics the core should be from the last five years (2021-2026) with foundational works kept.

## 2. Search strategy and limitations

- **Date of search:** 21.10.2026. **Tool:** web search; the sources were then selected manually by title and abstract.
- **Queries (examples):** smartphone GPS accuracy urban indoor; location spoofing detection mobile apps;
  mobile attendance system geofencing; face recognition attendance liveness anti-spoofing; employee monitoring
  remote work trust; employee monitoring biometric data GDPR; Kazakhstan personal data law biometric.
- **Selection:** relevance to WorkTrack requirements; peer-reviewed venues preferred; preprints and practitioner
  or legal commentary are marked as such.
- **Limitations of the review:**
  - Bibliographic databases (Scopus, IEEE Xplore, ACM DL) were not searched systematically; the search should be repeated there.
  - Findings below are based on abstracts and visible parts of the papers, not on a full reading; items marked
    **[verify]** must be checked against the full text before the defense.
  - Foundational works (R1 to R4) are older than five years and are used only for the qualitative picture;
    the core evidence is from 2022 to 2026 (R5 to R12).  - No work was found that studies periodic presence verification between check-in and check-out
    (see section 5, gap 1). Absence in the sources reviewed does not prove that no such work exists.

## 3. Source analysis table

Type: **A** peer-reviewed article or conference paper; **P** preprint (not peer-reviewed); **L** low weight
(student or competition paper, abstract only); **G** guidance, legal or practitioner commentary.
**F** marks foundational works older than five years.

| ID | Source | Year | Type | What it studies | Main findings (as far as visible in the abstract) | Limitations | Use in WorkTrack |
|---|---|---|---|---|---|---|---|
| R1 | Merry K., Bettinger P. Smartphone GPS accuracy study in an urban environment. PLOS ONE 14(7) | 2019 | A, F | Static horizontal positions of smartphones in an urban setting; effect of season, WiFi and tree cover | Earlier results on smartphone accuracy are mixed; accuracy depends on environment and connectivity | Static measurements, one city, few devices | Justifies accuracy handling and the design of EXP-3 |
| R2 | Kjaergaard M. B., Blunck H., Godsk T., Toftkjaer T., Christensen D. L., Gronbaek K. Indoor Positioning Using GPS Revisited. Pervasive 2010 | 2010 | A, F | Measurement campaign of indoor GPS reception in many buildings | With high-end receivers the RMS 2D error was below 5 m in wooden and below 10 m in most brick and concrete buildings; phone receivers had lower availability and accuracy; building materials and multipath explain differences | Old hardware; buildings in one country | Indoor accuracy depends on the building, so a fixed rejection rule is unsafe |
| R3 | Zandbergen P. A., Barbeau S. J. [verify authors]. Positional Accuracy of Assisted GPS Data from High-Sensitivity GPS-enabled Mobile Phones. Journal of Navigation 64(3), 381-399 | 2011 | A, F | A-GPS accuracy and availability on phones, indoors and outdoors | Indoor errors are larger than outdoor errors; valid fixes are mostly available; discusses the reported accuracy estimate as a probability-based (about 68%) measure | Phones from around 2009-2010 | The 68% interpretation of reported accuracy is the basis of the error model in EXP-2 |
| R4 | Zeng K. C., Shu Y., Liu S., Dou Y., Yang Y. A Practical GPS Location Spoofing Attack in Road Navigation Scenario. HotMobile 2017 | 2017 | A, F | A practical GPS spoofing attack against navigation | Shows that GPS spoofing is practically feasible [verify details in the abstract] | Signal-level attack in vehicles, not app-level mock location | Motivates the documented limit: no full protection against spoofing |
| R5 | Yun J., Lim C., Park B. Inherent Limitations of Smartphone GNSS Positioning and Effective Methods to Increase the Accuracy Utilizing Dual-Frequency Measurements. Sensors 22(24), 9879 | 2022 | A | Limits of smartphone GNSS and the benefit of dual-frequency measurements in open and urban areas | Smartphone GNSS has inherent limitations; the proposed method improved urban horizontal accuracy by 58.4% and reached about 20 m in a deep urban area (Seoul) | Needs dual-frequency devices and special processing not available in ordinary apps | Even enhanced phone positioning gives tens of metres in dense urban areas, so accuracy must be part of the decision |
| R6 | Retscher G., Weigert [verify authors]. Assessment of a dual-frequency multi-GNSS smartphone for surveying applications. Applied Geomatics 14, 765-784 | 2022 | A | Google Pixel 5 dual-frequency receiver against a geodetic receiver | Signal quality is lower than for a geodetic receiver; sub-half-metre results only with long observation; suitable for GIS and location-based services with moderate accuracy requirements | Long observation times and raw data processing; open-sky reference | Upper bound of what a modern phone can reach under good conditions; a single quick fix is worse |
| R7 | Geofencing in location-based behavioral research: Methodology, challenges, and implementation [verify authors]. Behavior Research Methods 56, 6411-6439 | 2024 | A | Geofencing on Android and iOS in three studies (controlled tests and real-world tests) | Sensitivity and precision depend on event type, radius, environment, operating system and user behaviour; too small a radius increases misses; a 10 m radius gave missed events on Android; with Wi-Fi enabled the minimum radius can be 20 to 50 m | Event-driven OS geofences for behavioural research, not on-demand attendance check-in | Guidance for geofence radius and the minimum radius recommendation (section 6); WorkTrack requests the location on demand |
| R8 | [verify title and authors]. Mindanao Journal of Science and Technology 23(2), 133-148 | 2025 | A | Geofence-based attendance system tested in five environments with a 20 m radius at several distances from the boundary | Authors report signal degradation of 43.35% indoors, a false positive rate of 2% and a strong reduction of proxy fraud in field tests | Self-reported figures; one setting; details not visible [verify] | Test design with points around the boundary for EXP-3; closest published example of an evaluated geofence attendance system |
| R9 | Rustamov A., Minetto A., Dovis F. Improving GNSS Spoofing Awareness in Smartphones via Statistical Processing of Raw Measurements. IEEE Open Journal of the Communications Society 4, 873-891 | 2023 | A | Vulnerability of Android phones to simple spoofing and detection from raw GNSS measurements | Phones are vulnerable to simplistic spoofing; a detection technique based on statistics of raw measurements (signal strength, pseudorange) is proposed for Android devices | Signal-level spoofing; needs the raw measurement API; not about mock-location apps | Application-level detection is possible but demanding: spoofing flag stays optional (FR-41) |
| R10 | Babic K., Balic M., Begusic D. [verify authors]. Recent Advances on Jamming and Spoofing Detection in GNSS. Sensors 24(13), 4210 | 2024 | A | Review of jamming and spoofing detection methods, including smartphones | Surveys detection methods and datasets; smartphone sensitivity to spoofing is visible in raw measurements | Receiver-level focus | State of the art of spoofing detection; supports the documented limitation |
| R11 | Idris M., Wijaya R., Wirayuda T. A. B. Employee Attendance System Based on Face Recognition and Liveness Detection Using MagFace. Indonesian Journal of Computing 11(1) [verify volume and issue; DOI contains v10i2] | 2026 | A | Face-based employee attendance with CNN liveness detection, FAR and FRR | Face recognition without liveness detection is vulnerable to photo, video and screen attacks; combining both is proposed | Laboratory evaluation; details not visible [verify] | In-app face recognition would need liveness detection, so FR-40 prefers device biometrics |
| R12 | [verify authors]. Performance consequences of automated workplace control. Frontiers in Behavioral Economics 4 | 2025 | A | Controlled experiment on algorithmic versus human control, performance and trust | Human control improved performance; algorithmic control did not harm it but did not improve it; higher control intensity showed no detrimental effect | Online experiment with a real-effort task | Automated checks do not raise performance by themselves: the system records events, people decide |
| R13 | Evaluating Smartphone GNSS Accuracy for Geofenced 6 GHz Operations, arXiv:2603.26706 [verify authors] | 2026 | P | Reported GNSS accuracy of Android phones outdoors and indoors, walking and driving | Indoor accuracy degrades substantially on all tested devices; make and model matter (outdoor medians below about 5 m for tested Samsung phones, worse for tested Pixel phones; indoor values of tens of metres) | Preprint; regulatory context, not attendance; few models | Device variability: EXP-3 on at least two devices |
| R14 | Guardian Positioning System (GPS) for Location Based Services, arXiv:2505.09743 [verify authors] | 2025 | P | Location spoofing detection for location-based apps using an extended RAIM approach | Reports 18-58% higher detection than platform location and network-based detection at 5-10% false positives; notes that apps cross-check GNSS, Wi-Fi and IP | Preprint; needs raw GNSS measurements | Platform-level checks are weaker than multi-source cross-checking |
| R15 | Zaihami M. A. S. et al. Biometric and Smart Identification Reader (B.A.S.I.R.): Combating Proxy Attendance via Multiprocessing Dual-Factor Authentication. PITRAM 2026, UiTM Selangor | 2026 | L | Proxy attendance in education: survey of 250 respondents and a fraud simulation | 94% of respondents had seen QR code sharing; proxy success 89.5% for QR, 36.4% for RFID, 8.2% for RFID with face verification and liveness | Student innovation competition paper; own survey and simulation; not independent evidence | Illustrates that a QR code alone is weak; FR-39 requires combination with geolocation |
| R16 | Thiel C., McClean S., Harvey J., Prince N. Surveilling Employees Erodes Trust and Puts Managers in a Bind. Harvard Business Review Digital Articles | 2024 | G | Two studies on monitoring and the supervisor-employee relationship (abstract) | Monitoring used for control lowered performance and raised deviant behaviour; monitoring used for developmental feedback preserved relationships and performance | Practitioner outlet, abstract only | Transparent purpose; checks are not punishment (NFR-05, FR-36) |
| R17 | UK Information Commissioner's Office. Employment practices: monitoring at work, draft guidance | 2022 | G | Employer obligations when monitoring workers, including biometric data for time and attendance | Monitoring must be necessary, proportionate and transparent; workers must be informed in advance; impact assessment and a condition for special-category data are expected for biometrics | UK jurisdiction; draft, check the final version | Minimisation and transparency (NFR-05) |
| R18 | A&O Shearman. EU and UK Data Protection Regulatory Trends 2024: employee surveillance and biometrics | 2024 | G | Enforcement trends of data protection authorities | Authorities act on workplace biometrics, treat them as special-category data and point to the imbalance between employer and employee | Law-firm commentary | Biometric confirmation stays optional (FR-40) |
| R19 | Baker McKenzie; Morgan Lewis. Kazakhstan: 2026 amendments to personal data legislation | 2026 | G | 2026 amendments to the Kazakhstan law on personal data | Law-firm summaries state that the facial image and the facial biometric template are among personal data identifiers; main provisions in force from July and August 2026 | Secondary sources; the law text must be checked (adilet.zan.kz) | Local legal context for optional biometrics and data minimisation |

**Counts:** 19 sources. Scientific publications R1 to R15: 12 peer-reviewed (R1 to R12), 2 preprints (R13, R14),
1 low-weight (R15). Guidance, legal and practitioner sources R16 to R19.
**Recency:** 8 peer-reviewed sources from 2022 to 2026 (R5 to R12), plus 2 preprints and 1 competition paper from 2025 to 2026,
and 4 guidance sources from 2022 to 2026. Foundational works older than five years: R1 to R4 (4 sources, used for the
qualitative picture only).

## 4. Critical review by theme

### 4.1 Smartphone positioning accuracy and geofences (R1 to R3, R5 to R8, R13)

Foundational studies (R1 to R3) establish the qualitative pattern: a few metres outdoors, more indoors, depending on the
building and the device. Recent work confirms it with current hardware. Even a method that improves urban accuracy
by more than half still ends at about 20 m in a deep urban area (R5); a modern dual-frequency phone reaches sub-metre accuracy
only with long observation and offline processing (R6); a 2026 preprint reports tens of metres indoors and clear
differences between models (R13). For geofences specifically, R7 shows that radius, operating system and environment
change the miss rate, and that very small radii cause misses; R8 is the closest example of an evaluated geofence attendance
system and tests points on both sides of the boundary, which is adopted in EXP-3. Limits of this evidence: R5 and R6 study
positioning precision with special processing, not the quick single fix an app receives; R7 studies event-driven OS geofences;
R8 has self-reported figures. Two design consequences follow: the accuracy reported by the device is part of the decision,
and thresholds are measured on the target devices (EXP-3) and not copied from papers.
The reported accuracy is interpreted as a probability-based estimate (R3); this is an assumption of the simulation (EXP-2)
and is checked against the documentation of the location API.

### 4.2 Location spoofing (R4, R9, R10, R14)

GPS spoofing is practically feasible (R4), and Android phones are vulnerable to simple spoofing (R9). Detection on phones is
an active research area (R9, R10), but the methods work on raw GNSS measurements and are aimed at signal-level attacks;
the common student-level attack (a mock-location app) is not covered by these papers. A 2025 preprint shows that simple
platform-level checks are weaker than cross-checking several sources (R14). The literature supports two decisions:
mock-location detection is only an additional flag (FR-41), and the requirements state that protection is not guaranteed.
Application-level mock-location detection is described in official Android documentation, which still has to be added.

### 4.3 Attendance systems and identity confirmation (R8, R11, R15)

Evaluated geofence attendance systems are rare in the sources found (R8). For identity confirmation, face recognition
without liveness detection can be defeated by photos and replays (R11), and QR codes are easily shared (R15, a student paper
reporting 89.5% proxy success for QR in its own simulation, so it is illustrative and not independent evidence).
This supports the planned order: geolocation first, additional methods optional, QR combined with location.

### 4.4 Monitoring, trust and remote work (R12, R16)

Experimental evidence suggests that automated control does not improve performance by itself (R12), and that monitoring seen as
control can lower performance and increase deviant behaviour, while developmental use does not (R16). The evidence is limited
(online experiments, a practitioner summary) and does not show that all monitoring is harmful. It supports choices already made:
no continuous tracking, no screen or keyboard monitoring, one failed check is not a violation, transparent purpose.

### 4.5 Privacy and law (R17 to R19)

Regulators in the UK and the EU treat workplace biometrics as high-risk, special-category data and ask for necessity,
proportionality and transparency (R17, R18). Commentary on the 2026 amendments in Kazakhstan lists facial images and facial
biometric templates among personal data identifiers (R19). For WorkTrack: location is collected only at the marks;
biometric confirmation is optional and, if implemented, preferably uses the device's own biometric check so that no face data
reaches the server; only fictional data in the repository. This is an engineering reading of the sources, not legal advice;
the law text must be checked.

## 5. Gaps and how the project responds

1. **Periodic presence verification between check-in and check-out.** No scientific study of it was found in the sources
   reviewed. Related areas that were not reviewed in detail: location proof schemes (witness-based and Wi-Fi-based proofs
   are cited in the reference lists of R-sources found by the search) and liveness checks. WorkTrack treats the mechanism as an own
   hypothesis and evaluates it (EXP-4) against a baseline.
2. **Effect of GPS error on attendance decisions.** R13 and R14 do not report it. WorkTrack evaluates it by simulation and a field
   test (EXP-2, EXP-3).
3. **Recent data on indoor accuracy.** Only one recent source (R2, a preprint). The field test provides local data.

## 6. Limitations taken into account in the project

| Limitation found in the literature | Decision in WorkTrack | Related requirements and tests |
|---|---|---|
| Error of tens of metres in dense urban areas and indoors, differs by device and building (R2, R5, R13) | Reported accuracy is part of the decision; uncertain marks get NEEDS_REVIEW; thresholds are measured; at least two devices in the field test | FR-14, FR-15, EXP-2, EXP-3 |
| Small radii cause misses; results depend on OS and environment (R7) | Recommend a minimum radius and warn the administrator below it; test points on both sides of the boundary (R8) | FR-06, EXP-3 |
| Reported accuracy is a probability-based estimate (R3) | Error model with a 68% radius in the simulation, to be confirmed in the API documentation | EXP-2 |
| Spoofing cannot be fully prevented; detection needs raw data and is demanding (R4, R9, R10, R14) | Mock-location flag is optional; limitation documented | FR-41, section 6 of the requirements |
| QR codes are shared easily (R15) | QR only combined with geolocation and server checks | FR-39 |
| Face recognition needs liveness detection and is legally sensitive (R11, R17 to R19) | Biometric confirmation optional; prefer device biometrics; no face data on the server | FR-40, NFR-05 |
| Control-oriented monitoring can harm trust (R12, R16) | No continuous tracking; one failed check is not a violation; transparent purpose | NFR-05, FR-36 |

Recommendation arising from R7 and the indoor errors, not yet a requirement: warn the administrator when a geofence radius is
smaller than the typical indoor error (for example below 50 m), and use 100 m as the default. To be decided after the field test.

## 7. References

Entries marked [verify] need authors, venue, volume, pages and DOI checked against the publisher page.

- [R1] Merry K., Bettinger P. Smartphone GPS accuracy study in an urban environment. PLOS ONE 14(7): e0219890, 2019. https://doi.org/10.1371/journal.pone.0219890
- [R2] Kjaergaard M. B., Blunck H., Godsk T., Toftkjaer T., Christensen D. L., Gronbaek K. Indoor Positioning Using GPS Revisited. Pervasive Computing, 2010 [verify pages]. https://archive.cs.helsinki.fi/www.cs.helsinki.fi/node/7537.html
- [R3] Zandbergen P. A., Barbeau S. J. [verify]. Positional Accuracy of Assisted GPS Data from High-Sensitivity GPS-enabled Mobile Phones. The Journal of Navigation 64(3): 381-399, 2011. https://doi.org/10.1017/S0373463311000051
- [R4] Zeng K. C., Shu Y., Liu S., Dou Y., Yang Y. A Practical GPS Location Spoofing Attack in Road Navigation Scenario. HotMobile 2017. https://doi.org/10.1145/3032970.3032983
- [R5] Yun J., Lim C., Park B. Inherent Limitations of Smartphone GNSS Positioning and Effective Methods to Increase the Accuracy Utilizing Dual-Frequency Measurements. Sensors 22(24): 9879, 2022. https://www.mdpi.com/1424-8220/22/24/9879
- [R6] Retscher G., Weigert [verify authors]. Assessment of a dual-frequency multi-GNSS smartphone for surveying applications. Applied Geomatics 14: 765-784, 2022 [verify DOI].
- [R7] Geofencing in location-based behavioral research: Methodology, challenges, and implementation [verify authors]. Behavior Research Methods 56: 6411-6439, 2024. https://link.springer.com/article/10.3758/s13428-023-02213-2
- [R8] [verify title and authors]. Mindanao Journal of Science and Technology 23(2): 133-148, 2025. https://mjst.ustp.edu.ph/index.php/mjst/article/download/2478/348
- [R9] Rustamov A., Minetto A., Dovis F. Improving GNSS Spoofing Awareness in Smartphones via Statistical Processing of Raw Measurements. IEEE Open Journal of the Communications Society 4: 873-891, 2023 [verify DOI].
- [R10] Babic K., Balic M., Begusic D. [verify]. Recent Advances on Jamming and Spoofing Detection in GNSS. Sensors 24(13): 4210, 2024. https://doi.org/10.3390/s24134210
- [R11] Idris M., Wijaya R., Wirayuda T. A. B. Employee Attendance System Based on Face Recognition and Liveness Detection Using MagFace. Indonesian Journal of Computing, 2026 [verify volume and issue]. https://doi.org/10.21108/indojc.v10i2.10294
- [R12] [authors, verify]. Performance consequences of automated workplace control. Frontiers in Behavioral Economics 4, 2025. https://doi.org/10.3389/frbhe.2025.1647057
- [R13] Evaluating Smartphone GNSS Accuracy for Geofenced 6 GHz Operations. arXiv:2603.26706, 2026 [verify authors]. https://arxiv.org/abs/2603.26706
- [R14] Guardian Positioning System (GPS) for Location Based Services. arXiv:2505.09743, 2025 [verify authors]. https://arxiv.org/abs/2505.09743
- [R15] Zaihami M. A. S. et al. Biometric and Smart Identification Reader (B.A.S.I.R.): Combating Proxy Attendance via Multiprocessing Dual-Factor Authentication. PITRAM 2026, UiTM Cawangan Selangor, Kampus Dengkil, 2026.
- [R16] Thiel C., McClean S., Harvey J., Prince N. Surveilling Employees Erodes Trust and Puts Managers in a Bind. Harvard Business Review Digital Articles, 2024.
- [R17] Information Commissioner's Office. Employment practices: monitoring at work, draft guidance, 12 October 2022 (check for the final version). https://cy.ico.org.uk/media2/migrated/4021868/draft-monitoring-at-work-20221011.pdf
- [R18] A&O Shearman. EU and UK Data Protection Regulatory Trends so far in 2024: employee surveillance and biometrics. https://www.aoshearman.com/en/insights/ao-shearman-on-data/eu-and-uk-data-protection-regulatory-trends-so-far-in-2024-a-focus-on-employee-surveillance
- [R19] Baker McKenzie. Kazakhstan: new amendments to personal data legislation, July 2026. https://www.bakermckenzie.com/en/insight/publications/2026/07/kazakhstan-new-amendments-to-personal-data-legislation ; Morgan Lewis. Republic of Kazakhstan Data Protection Law: Amendments in 2026. https://www.morganlewis.com/pubs/2026/07/republic-of-kazakhstan-data-protection-law-amendments-in-2026

**To add before the defense:** official Android documentation of the location accuracy value and of mock-location detection;
the text of the Kazakhstan law on personal data (adilet.zan.kz); optionally Hu J., Yi D., Bisnath S., "A Comprehensive Analysis
of Smartphone GNSS Range Errors in Realistic Environments", Sensors 23: 1631, 2023 (read first, then decide).