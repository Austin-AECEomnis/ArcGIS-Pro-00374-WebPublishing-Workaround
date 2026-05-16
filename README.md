# ArcGIS Pro 3.7 — Web Layer Publishing: Error Code 00374 Troubleshooting & Verified Workaround
Documents a reproducible error 00374 failure encountered when publishing geoprocessing outputs from ArcGIS Pro 3.7 to ArcGIS Online as hosted feature layers, including eight attempted resolutions and a verified shapefile ZIP upload workaround. Reference guide for GIS professionals encountering the same publishing failure condition.
Documented By: Austin Berlin (AECE Omnis LLC | GIS Portfolio Development)
Date: May 15, 2026
Environment: ArcGIS Pro 3.7 | Personal Use License | Windows 11 | ArcGIS Online Organizational Account
Status: Resolved — Verified Workaround Documented

# 🛡️ Researcher's Statement
This repository documents a reproducible publishing failure encountered while attempting to share a locally generated geoprocessing output from ArcGIS Pro 3.7 to ArcGIS Online as a hosted feature layer. The error persisted across multiple standard resolution attempts before a verified workaround was identified. This documentation is intended to provide a clear, step-by-step troubleshooting reference for GIS professionals encountering the same failure condition.

# 📊 Issue Metadata
**Error Code:** 00374  
**Severity:** High  
**Tool Used:** Share as Web Layer (ArcGIS Pro 3.7)  
**Source Layer Type:** Geoprocessing output — Summarize Within  
**Affected System:** ArcGIS Online Hosted Feature Layer publishing  
**Resolution:** Verified workaround — shapefile ZIP upload via ArcGIS Online Content

# 📝 Executive Summary
When attempting to publish a Summarize Within geoprocessing output from ArcGIS Pro 3.7 to ArcGIS Online as a hosted feature layer, error code 00374 was returned during the Analyze phase of the Share as Web Layer dialog. The error indicated that unique numeric IDs were not properly assigned to the feature class, preventing publication despite a fully populated OBJECTID field being visible in the attribute table.
Standard resolution methods including Copy Features, Feature Class to Feature Class, Repair Geometry, Calculate Field, and Add Auto Increment ID all failed to resolve the underlying ID registration issue. The verified solution bypassed the Pro publishing dialog entirely through a direct shapefile ZIP upload to ArcGIS Online.

# ⚙️ Technical Description
## 3.1 Background
ArcGIS Online's hosted feature layer hosting requirements enforce strict unique numeric ID registration at the geodatabase level. While ArcGIS Pro can display and analyze feature classes with OBJECTID fields locally, geoprocessing tool outputs, particularly Summarize Within, can produce feature classes where the ID field is populated but not registered in a manner that satisfies Online's hosting validation. The Share as Web Layer Analyze function detects this discrepancy and returns error 00374 as a high severity blocking error.
## 3.2 Why Standard Fixes Failed
The OBJECTID field in ArcGIS Pro is a system-managed field protected from manual editing. Standard field calculation tools including Calculate Field (Python and Arcade expression types) and Add Auto Increment ID (unlicensed at Personal Use tier) cannot access or modify it. Copy Features and Feature Class to Feature Class propagate the same underlying registration state rather than rebuilding it. Repair Geometry addresses spatial integrity but does not touch ID field registration.
## 3.3 Secondary Issue — Naming Conflicts (Error 000733)
Repeated geoprocessing attempts produced error 000733 (output feature class same as input) due to lingering selected features from prior analysis sessions being carried into new tool runs. This was resolved by clearing all active selections before each geoprocessing operation.

# 🚀 Steps to Reproduce
1. Run Summarize Within in ArcGIS Pro against a locally stored polygon feature class
2. Attempt to share the output as a Web Layer via Share tab — Share as Web Layer
3. Click Analyze
4. Observe error code 00374 — Unique numeric IDs are not assigned — returned at High severity with no Auto-Fix button available

# 🔍 Attempted Resolutions & Results
**Copy Features:** 00374 persists — ID registration state copied  
**Feature Class to Feature Class:** 000733 naming conflict — selection not cleared  
**Calculate Field — Python (!OBJECTID!):** 000539 — OBJECTID inaccessible to field calculator  
**Calculate Field — Arcade ($feature.OBJECTID):** 002717 — OBJECTID not found in Arcade context  
**Add Auto Increment ID:** Tool not licensed at Personal Use tier  
**Repair Geometry:** Completed successfully — 00374 still persists  
**Feature Class to Shapefile:** Completed — 00374 still returned in Share dialog  
**Feature Class to GDB (alternate location):** Completed — 00374 still returned in Share dialog

# ✅ Verified Workaround
## Step 1 — Clear all active selections
Before any export operation, right-click the source layer in the Contents panel, go to Selection and select Clear Selection. Confirm nothing is highlighted on the map canvas before proceeding.
## Step 2 — Export to Shapefile
Run Feature Class to Shapefile in the Geoprocessing pane. Point the output to a clean folder — C:\GIS\Outputs. Verify the following component files are present in the output folder after completion: .shp, .dbf, .shx, .prj, .cpg, .sbn, .sbx. Note: the .shx file is required and may be absent if the export is interrupted or run while a selection is active. If .shx is missing, clear the output folder and re-run.
## Step 3 — Create ZIP archive
In File Explorer select all shapefile component files directly — do not select the containing folder. Right-click and compress to ZIP. Open the ZIP to verify the component files are at the root level with no subfolder wrapping. ArcGIS Online will reject the upload if files are nested inside a subfolder.
## Step 4 — Upload to ArcGIS Online
Navigate to your ArcGIS Online Content page. Select New Item, choose From your computer, and upload the ZIP file. When prompted select Hosted Feature Layer as the item type. Complete the metadata fields and publish.
## Step 5 — Add to map
Open your target map in ArcGIS Online Map Viewer. Click Add, select My Content, locate the newly published hosted feature layer and add it to the map. Apply symbology as needed.

# 🛡️ Key Takeaways
- **Error 00374 in the Share as Web Layer dialog does not always indicate a true data integrity problem** — it can reflect an ID registration discrepancy specific to how certain geoprocessing tools build their output feature classes  
- **The OBJECTID field in ArcGIS Pro is protected from manual editing** and cannot be modified through standard field calculation tools  
- **The Add Auto Increment ID tool that would directly resolve 00374 is not available at the Personal Use license tier**  
- **Direct shapefile ZIP upload to ArcGIS Online bypasses Pro's publishing validation entirely** and is a reliable alternative publishing route for geoprocessing outputs  
- **Always clear active selections before running any export or geoprocessing operation** to avoid naming conflicts and incomplete feature exports

# 📅 Timeline
May 15, 2026: Error first encountered during Summarize Within output publishing — KendallCounty FloodAnalysis project
May 15, 2026: Eight resolution attempts documented across multiple geoprocessing and field calculation approaches
May 15, 2026: Shapefile ZIP upload workaround verified and confirmed — hosted feature layer successfully published to ArcGIS Online

# 🤝 Contact
Austin Berlin | AECE Omnis LLC
LinkedIn: linkedin.com/in/austinberlin
ArcGIS Portfolio: aeceomnis.maps.arcgis.com
