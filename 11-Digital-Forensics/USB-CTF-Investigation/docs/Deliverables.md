# 📋 Digital Forensics Investigation Deliverables

[cite_start]This document contains the official forensic deliverables and documentation for the USB Digital Forensics CTF Investigation (Case: `cartel.img`)[cite: 1, 27]. It details evidence verification, volume triage, digital artifact discovery, deleted data carving, timeline reconstruction, tool logs, and execution commands compiled during the forensic analysis.

---

## 📑 Table of Contents

- [Task 1 — Evidence Verification](#task-1--evidence-verification)
- [Task 2 — Initial Triage](#task-2--initial-triage)
- [Task 3 — Evidence Discovery](#task-3--evidence-discovery)
- [Task 4 — Deleted Data Analysis](#task-4--deleted-data-analysis)
- [Task 5 — Timeline Reconstruction](#task-5--timeline-reconstruction)
- [📸 Screenshots](#-screenshots)
- [📂 Recovered Files](#-recovered-files)
- [🛠️ Tools Used](#%EF%B8%8F-tools-used)
- [💻 Commands Executed](#-commands-executed)
- [📌 Assignment Checklist](#-assignment-checklist)
- [📎 Related Documents](#-related-documents)

---

# Task 1 — Evidence Verification

### Hash Verification Results

* [cite_start]**MD5 Hash:** `80348c58eec4c328ef1f7709adc56a54` 
* [cite_start]**SHA-256 Hash:** `ce550124200a997c61b413941cbef4df9619a2f96579674952294a176a32be65` 

> [cite_start]**Note:** Hashes were derived directly from the source image file (`cartel.img`) prior to examination[cite: 5, 37].

### Why Integrity Verification is Performed

[cite_start]Establishing cryptographic hash values immediately upon receipt of digital evidence establishes an unalterable forensic baseline[cite: 5, 37]. [cite_start]This process ensures that no data has been modified, corrupted, or altered by analysis tools during examination, preserving the chain of custody and legal integrity of the digital evidence[cite: 4, 30].

### Significance of Matching Hash Values

Matching hash values verify mathematically that the forensic copy being analyzed is bit-for-bit identical to the master image acquired at the time of seizure. Any deviation in even a single bit would generate a completely different hash string, signaling evidence contamination or tampering.

---

# Task 2 — Initial Triage

| Triage Parameter | Forensic Observation | Supporting Reference |
| :--- | :--- | :--- |
| **Image Format** | Raw image container (`raw`) | [cite_start]Sleuth Kit `img_stat` output [cite: 9, 48] |
| **File System Type** | `FAT16` | [cite_start]Sleuth Kit `mmls` / PhotoRec output [cite: 9, 47, 51] |
| **Number of Partitions** | 1 Partition | [cite_start]Volume layout analysis [cite: 9, 47, 51] |
| **Storage Characteristics** | **Total Size:** $259,506,176\text{ bytes}$ ($259\text{ MB}$ / $247\text{ MiB}$)<br>**Sector Size:** $512\text{ bytes}$ | [cite_start]`img_stat` / PhotoRec metadata [cite: 9, 48, 51] |

### Importance of Initial Triage

Initial triage provides a non-destructive preliminary survey of the digital evidence. [cite_start]By identifying partition layouts, sector sizes, and file system architectures prior to deep analysis, examiners can select appropriate toolsets, calculate offset parameters correctly, and structure an optimal investigative strategy without risking evidence alteration[cite: 8, 30, 46].

---

# Task 3 — Evidence Discovery

### Artifact 1: Microsoft Word Document (`00335017.doc`)

* [cite_start]**Description:** A deleted Microsoft Word text document containing 1,022 words[cite: 11, 20, 55].
* [cite_start]**Location Found:** Recovered from unallocated clusters into directory `/recovered/doc/00335017.doc`[cite: 55, 56].
* [cite_start]**Discovery Method:** Data carving via PhotoRec/Foremost followed by terminal path location using `find recovered`[cite: 5, 34, 55].
* [cite_start]**Why It Is Relevant:** Directly refutes the suspect's verbal claim that the USB media contained exclusively personal photographs[cite: 20, 70].
* [cite_start]**Supporting Evidence:** Author metadata field identified as `"NOWAY MAN"`; document rendered and validated in LibreOffice Writer[cite: 12, 13, 56, 57].
* [cite_start]**Screenshot Reference:** Figure 7, Figure 8 [cite: 12, 13, 56, 57]

---

### Artifact 2: Text Document — Gumbo Recipe 1 (`gumbo1.txt`)

* [cite_start]**Description:** Plain text file containing a recipe titled *"SHRIMP AND TASSO GUMBO"*[cite: 23, 74].
* [cite_start]**Location Found:** File system root allocation / Inode `4`[cite: 23, 60, 74].
* [cite_start]**Discovery Method:** Sleuth Kit manual inode extraction using `icat cartel.img 4`[cite: 23, 74].
* [cite_start]**Why It Is Relevant:** Confirms active usage of text format storage within the file allocation table[cite: 21, 71].
* [cite_start]**Supporting Evidence:** Full ingredient and preparation text stream output to standard console[cite: 23, 74].
* [cite_start]**Screenshot Reference:** Figure 7 (icat Inode 4), Figure 9, Figure 10 [cite: 14, 15, 23, 59, 60, 74]

---

### Artifact 3: Text Document — Gumbo Recipe 2 (`gumbo2.txt`)

* [cite_start]**Description:** Plain text file containing a recipe titled *"SHRIMP AND ANDOUILLE SAUSAGE GUMBO"*[cite: 24, 75].
* [cite_start]**Location Found:** File system root allocation / Inode `6`[cite: 24, 60, 75].
* [cite_start]**Discovery Method:** Sleuth Kit manual inode extraction using `icat cartel.img 6`[cite: 24, 75].
* [cite_start]**Why It Is Relevant:** Secondary corroborating textual artifact establishing user file management habits[cite: 21, 71].
* [cite_start]**Supporting Evidence:** Console output containing complete text and publication citation (*Bon Appetit, November 1992*)[cite: 24, 75].
* [cite_start]**Screenshot Reference:** Figure 8 (icat Inode 6), Figure 10 [cite: 15, 24, 60, 75]

---

### Artifact 4: Graphic Image File (`f0104057.jpg`)

* [cite_start]**Description:** Carved image file displaying a black-and-white sketch of a rhinoceros[cite: 15, 61, 62].
* [cite_start]**Location Found:** Carved output path `recup_dir.1/f0104057.jpg`[cite: 51, 61].
* [cite_start]**Discovery Method:** Unallocated space data carving via PhotoRec 7.1[cite: 10, 34, 51].
* [cite_start]**Why It Is Relevant:** Confirms the presence of graphical image assets carved from non-contiguous clusters[cite: 19, 69].
* [cite_start]**Supporting Evidence:** Successfully rendered image via system viewer `xdg-open`[cite: 61, 62].
* [cite_start]**Screenshot Reference:** Figure 11 [cite: 15, 62]

---

### Artifact 5: Photographic Image File (`f0106865.gif`)

* [cite_start]**Description:** Carved GIF image depicting an alligator in a natural environment[cite: 15, 61, 62].
* [cite_start]**Location Found:** Carved output path `recup_dir.1/f0106865.gif`[cite: 51, 61].
* [cite_start]**Discovery Method:** Unallocated space data carving via PhotoRec 7.1[cite: 10, 34, 51].
* [cite_start]**Why It Is Relevant:** Verifies media carving capability for photographic files[cite: 19, 69].
* [cite_start]**Supporting Evidence:** Visual rendering in system image viewer window[cite: 61, 62].
* [cite_start]**Screenshot Reference:** Figure 11 [cite: 15, 62]

---

# Task 4 — Deleted Data Analysis

| Original Filename | Recovered Filename | File Type | Recovery Method | Significance |
| :--- | :--- | :--- | :--- | :--- |
| `She_died_in_February...doc` | `00335017.doc` | MS Word Document (`.doc`) | PhotoRec / Foremost Carving | [cite_start]Contains 1,022 words; authored by "NOWAY MAN"; falsifies suspect's statement[cite: 11, 12, 20, 55, 56]. |
| `gumbo1.txt` | `gumbo1.txt` | Plain Text (`.txt`) | Sleuth Kit `icat` (Inode 4) | [cite_start]Extracted directly from Inode 4; proves text file creation and access[cite: 23, 60, 74]. |
| `gumbo2.txt` | `gumbo2.txt` | Plain Text (`.txt`) | Sleuth Kit `icat` (Inode 6) | [cite_start]Extracted directly from Inode 6; corroborates textual data storage[cite: 24, 60, 75]. |

### Data Recovery Process Summary

[cite_start]The recovery process utilized both automated file carving (PhotoRec 7.1 and Foremost) and low-level file system extraction (Sleuth Kit `icat`)[cite: 5, 23, 34, 74]. [cite_start]PhotoRec carved 132 files from unallocated space[cite: 10, 52]. [cite_start]The successful extraction of a 1,022-word Microsoft Word document directly invalidates the suspect's claim that the drive held only personal photos[cite: 20, 70].

---

# Task 5 — Timeline Reconstruction

| Timestamp | Event Type | File / Artifact Involved | Forensic Interpretation |
| :--- | :--- | :--- | :--- |
| **Fri Apr 30 2004 00:00:00** | File Access (`.a..`) | `/gumbo1.txt` (Inode 4)<br>`/gumbo2.txt` (Inode 6) | [cite_start]System read access logged for both text recipe files[cite: 64, 65]. |
| **Fri Apr 30 2004 18:11:20** | Modification / Metadata (`m..b`) | `/gumbo1.txt` (Inode 4) | [cite_start]File content modified and metadata updated in FAT table[cite: 64, 65]. |
| **Fri Apr 30 2004 18:11:24** | Modification / Metadata (`m..b`) | `/gumbo2.txt` (Inode 6) | [cite_start]Secondary file modification and metadata update recorded[cite: 64, 65]. |
| **Aug 2 2026 09:15 AM** | Evidence Seizure | Physical USB Device | Evidence seized by EFCC officers during warrant execution. |
| **Aug 3 2026** | Forensic Examination | `cartel.img` | [cite_start]Evidence hashed, carved, and analyzed by Examiner Okirie Chukwumeniem Faith[cite: 1, 5, 27, 37]. |

### Timeline Summary Findings

[cite_start]Timeline parsing reveals concentrated user file activity occurring on **April 30, 2004**, between 00:00:00 and 18:11:24 UTC/Local. [cite_start]This demonstrates active modification and access to text-based documents prior to drive acquisition[cite: 21, 71].

### Timeline Limitations

* [cite_start]**Absence of Specific Deletion Timestamps:** The parsed bodyfile output does not contain explicit deletion timestamp markers for carved unallocated files[cite: 64, 65].
* **System Clock Baseline:** No hardware reference clock calibration data is available in the provided evidence documentation to confirm time zone offsets for April 2004 logs.

---

# 📸 Screenshots

Below is the index of forensic screenshots documented during the examination:

1. [cite_start]**Figure 1 – MD5 Hash Verification:** Console execution of `md5sum cartel.img` displaying hash `80348c58eec4c328ef1f7709adc56a54`[cite: 6, 36, 38].
2. [cite_start]**Figure 2 – SHA-256 Hash Verification:** Console execution of `sha256sum cartel.img` displaying hash `ce550124200a997c61b413941cbef4df9619a2f96579674952294a176a32be65`.
3. [cite_start]**Figure 3 – MMLS Partition Analysis:** Execution of `mmls cartel.img` establishing FAT16 partition schema[cite: 9, 46, 47].
4. [cite_start]**Figure 4 – Image Metadata Summary:** Execution of `img_stat cartel.img` showing raw image size ($259,506,176\text{ bytes}$) and sector geometry ($512\text{ bytes}$).
5. [cite_start]**Figure 5 – PhotoRec Recovery:** Screen capture showing 132 files extracted into `recup_dir`[cite: 10, 50, 51, 52].
6. [cite_start]**Figure 6 – Foremost Recovery:** Console output showing redundant carving execution via Foremost[cite: 6, 7, 41, 43].
7. [cite_start]**Figure 7 – Metadata Analysis & Manual Carving (icat):** Terminal showing metadata analysis (Author: NOWAY MAN) and `icat` extraction for Inode 4 (`gumbo1.txt`)[cite: 12, 23, 55, 56, 74].
8. [cite_start]**Figure 8 – Visual Verification & Manual Carving (icat):** Visual rendering of `00335017.doc` in LibreOffice Writer alongside `icat` extraction for Inode 6 (`gumbo2.txt`)[cite: 13, 24, 56, 57, 75].
9. [cite_start]**Figure 9 – istat Directory Analysis:** Detailed inode attribute analysis for `gumbo1.txt`[cite: 14, 58, 59].
10. [cite_start]**Figure 10 – Tail of Recovered Text Content:** Sleuth Kit `fls` bodyfile generation log[cite: 15, 59, 60].
11. [cite_start]**Figure 11 – Recovered Multimedia Assets:** Viewer display of carved graphics `f0104057.jpg` and `f0106865.gif`[cite: 15, 61, 62].
12. [cite_start]**Figure 12 – Detailed Chronological Log:** Output of `mactime -b bodyfile.txt > timeline.txt`[cite: 17, 63, 64, 65].

---

# 📂 Recovered Files

[cite_start]A total of **132 files** were recovered from unallocated clusters and allocated directory structures of `cartel.img`[cite: 10, 19, 52, 69]. Key analyzed artifacts include:

* [cite_start]**1x Microsoft Word Document:** `00335017.doc` ($1,022\text{ words}$, Author: `"NOWAY MAN"`)[cite: 11, 12, 20, 56].
* [cite_start]**2x Plain Text Files:** `gumbo1.txt` (Inode 4) and `gumbo2.txt` (Inode 6)[cite: 23, 24, 60, 74, 75].
* [cite_start]**Multiple Image/Graphic Assets:** Carved JPEG (`f0104057.jpg`) and GIF (`f0106865.gif`) graphics[cite: 15, 61, 62].
* [cite_start]**System/Text Logs:** `audit.txt` located within carved directory structures[cite: 55, 56].

---

# 🛠️ Tools Used

| Tool Name | Forensic Function / Purpose |
| :--- | :--- |
| **SIFT Workstation** | [cite_start]Specialized Linux forensic workstation environment[cite: 5, 33]. |
| **`md5sum` / `sha256sum`** | [cite_start]Cryptographic hash calculation and integrity verification[cite: 5, 6, 36, 38, 39]. |
| **`mmls`** | [cite_start]Displays partition layout and structure of the disk image[cite: 5, 9, 34, 47]. |
| **`img_stat`** | [cite_start]Displays image file details, byte counts, and sector geometry[cite: 5, 9, 34, 48]. |
| **`fls`** | [cite_start]Lists file names and directory entries (including deleted entries)[cite: 5, 15, 34, 60]. |
| **`istat`** | [cite_start]Displays inode metadata structures and block allocations[cite: 5, 14, 34, 59]. |
| **`icat`** | [cite_start]Extracts file contents based on inode numbers[cite: 5, 23, 24, 34, 74, 75]. |
| **`mactime`** | [cite_start]Parses bodyfile data to generate chronological activity timelines[cite: 17, 57, 65]. |
| **PhotoRec 7.1** | [cite_start]Signature-based unallocated space data carving utility[cite: 5, 10, 34, 51]. |
| **Foremost** | [cite_start]Redundant file carving utility used for verification[cite: 5, 6, 34, 41, 43]. |
| **LibreOffice Writer** | [cite_start]Application viewer for Word document visual content verification[cite: 5, 13, 35, 57]. |

---

# 💻 Commands Executed

```bash
# ==========================================
# 1. EVIDENCE INTEGRITY VERIFICATION
# ==========================================
md5sum cartel.img
sha256sum cartel.img

# ==========================================
# 2. FILE SYSTEM & PARTITION TRIAGE
# ==========================================
mmls cartel.img
img_stat cartel.img

# ==========================================
# 3. UNALLOCATED SPACE DATA CARVING
# ==========================================
# Carving using PhotoRec 7.1
photorec cartel.img

# Redundant carving using Foremost
foremost -i cartel.img -o recovered/

# Locate carved document artifacts
find recovered -name "*.doc"

# ==========================================
# 4. SLEUTH KIT ARTIFACT EXTRACTION
# ==========================================
# Inspect directory inode details for gumbo1.txt
istat cartel.img 4

# Extract content of Inode 4 (Shrimp and Tasso Gumbo)
icat cartel.img 4

# Extract content of Inode 6 (Shrimp and Andouille Sausage Gumbo)
icat cartel.img 6

# ==========================================
# 5. TIMELINE GENERATION & RECONSTRUCTION
# ==========================================
# Generate bodyfile from file system metadata
fls -r -m / cartel.img > bodyfile.txt

# Inspect raw bodyfile structure
cat bodyfile.txt

# Parse bodyfile into human-readable timeline
mactime -b bodyfile.txt > timeline.txt

# Display generated timeline log
cat timeline.txt