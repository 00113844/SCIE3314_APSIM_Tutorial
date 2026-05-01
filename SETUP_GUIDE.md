# SCIE3314 APSIM Wiki — Setup Guide

**Date**: May 1, 2026  
**Status**: ✅ Local setup complete; ready for GitHub push

---

## What's Been Created

### Folder Structure
```
SCIE3314_APSIM_Wiki/
├── CLAUDE.md                           # Schema + workflows (READ THIS FIRST)
├── README.md                           # Project overview
├── .gitignore                          # Git configuration
├── raw/
│   ├── sources/                        # (empty; ready for docs/papers)
│   ├── exercises/                      # (empty; ready for .apsimx files)
│   └── instructor_notes/               # (empty; ready for Q&A logs)
├── wiki/
│   ├── index.md                        # Master catalog
│   ├── log.md                          # Ingest & query log
│   ├── entities/
│   │   ├── Wheat.md                    # ✅ Bootstrapped
│   │   ├── ESW_Extractable_Soil_Water.md # ✅ Bootstrapped
│   │   ├── Thermal_Time.md             # ✅ Bootstrapped
│   │   ├── Water_Balance.md            # ✅ Bootstrapped
│   │   └── [9 more entity stubs]       # (templates ready in index.md)
│   ├── concepts/                       # (ready for scaffolding, near-solved, etc.)
│   ├── sources/                        # (ready for source summaries)
│   ├── synthesis/                      # (ready for cross-cutting analysis)
│   ├── faq/                            # (ready for Q&A)
│   └── exercises/                      # (ready for module/exercise guides)
└── image_placeholders/                 # (empty; ready for diagram specs)
```

### Key Files Created

| File | Purpose |
|------|---------|
| **CLAUDE.md** | Schema, page templates, workflows, conventions |
| **README.md** | Project overview; for GitHub landing page |
| **.gitignore** | Standard Git ignore (Python, IDE, OS files) |
| **wiki/index.md** | Master catalog of all wiki pages |
| **wiki/log.md** | Append-only ingest/query log |
| **wiki/entities/Wheat.md** | Crop model entity page |
| **wiki/entities/ESW_Extractable_Soil_Water.md** | Soil water entity page |
| **wiki/entities/Thermal_Time.md** | GDD/phenology entity page |
| **wiki/entities/Water_Balance.md** | Daily water accounting entity page |

---

## Next Steps: Push to GitHub

### Step 1: Create GitHub Repository

1. Go to **https://github.com/new**
2. **Repository name**: `SCIE3314_APSIM_Tutorial`
3. **Description**: `LLM-powered knowledge base for APSIM crop modeling education`
4. **Visibility**: 
   - **Private** (recommended if UWA-internal)
   - **Public** (if sharing with broader community)
5. **Initialize**: Do NOT add README/License (you already have them)
6. Click **Create repository**

### Step 2: Add Remote & Push Locally

Open PowerShell in the `SCIE3314_APSIM_Wiki` folder:

```powershell
# Navigate to the wiki folder
cd "C:\Users\00113844\Documents\SCIE3314_APSIM_Tutorial\SCIE3314_APSIM_Wiki"

# Initialize Git (if not already done)
git init

# Add all files
git add .

# Initial commit
git commit -m "Initial commit: LLM Wiki structure + bootstrap entities (Wheat, ESW, Thermal_Time, Water_Balance)"

# Add GitHub remote (replace [USERNAME] with your GitHub username/org)
git remote add origin https://github.com/[USERNAME]/SCIE3314_APSIM_Tutorial.git

# Push to GitHub
git branch -M main
git push -u origin main
```

### Step 3: Verify on GitHub

1. Go to **https://github.com/[USERNAME]/SCIE3314_APSIM_Tutorial**
2. Check that all files are visible
3. README.md should render on the landing page
4. Folder structure visible in browser

---

## Understanding the Schema

Read **CLAUDE.md** in this order:

1. **Overview** (top section) — Understand "three-layer architecture"
2. **Operations** — How to ingest sources, answer queries, lint
3. **Conventions** — Wikilinks, frontmatter, naming
4. **Page Templates** — Use these when creating new pages
5. **Tips & Tricks** — Optional but helpful

---

## Starting the Ingest Workflow

### Example: Ingest Your APSIM_Internal_Guide

1. **Copy** `REFERENCE_DOCS/APSIM_Internal_Guide.md` to `raw/sources/`
   ```powershell
   cp "../REFERENCE_DOCS/APSIM_Internal_Guide.md" "raw/sources/"
   ```

2. **Tell me**: "I've added APSIM_Internal_Guide to raw/sources/"

3. **I will**:
   - Read the guide
   - Create/update entity pages (Soil, Manager, Clock, etc.)
   - Create `wiki/sources/Guide_APSIM_Internal_Summary.md`
   - Update `wiki/index.md`
   - Add entry to `wiki/log.md`

4. **You will**: Review updates and commit

5. **Commit & Push**:
   ```powershell
   git add wiki/
   git commit -m "Ingest: APSIM Internal Guide → entity pages (Soil, Manager, Clock, etc.)"
   git push
   ```

---

## Workflow for Adding Exercise Files

### Example: Add `wheat_nitrogen_factorial.apsimx` to exercises

1. **Copy** `.apsimx` file to `raw/exercises/`
   ```powershell
   cp "wheat_nitrogen_factorial.apsimx" "raw/exercises/"
   ```

2. **Create companion notes** (optional but helpful):
   ```powershell
   # File: raw/exercises/wheat_nitrogen_factorial_design.md
   # Content: Explains why this file was created, what it models, expected results
   ```

3. **Tell me**: "I've added wheat_nitrogen_factorial.apsimx to raw/exercises/"

4. **I will**:
   - Review the `.apsimx` design
   - Create/update `wiki/exercises/Exercise_B_Nitrogen_Response_Guide.md`
   - Link to this `.apsimx` file in the guide
   - Update `wiki/index.md`

5. **Commit & Push**:
   ```powershell
   git add raw/exercises/ wiki/exercises/
   git commit -m "Add: wheat_nitrogen_factorial.apsimx; create Exercise_B guide"
   git push
   ```

---

## First Teaching Cycle: Collecting Q&A

As you teach:

1. **Collect** common student questions
2. **Add to** `raw/instructor_notes/Common_Questions_Module_[X].md`
   ```markdown
   # Common Questions — Module 2

   ## Q: Why doesn't rainfall increase ESW every time?
   A: Because of runoff. If soil is already saturated, rain can't infiltrate.

   ## Q: Can ESW go negative?
   A: No, it stops at LL (lower limit).
   ```

3. **Tell me**: "I've added Common_Questions_Module_2 to instructor_notes/"

4. **I will**:
   - Extract key concepts from Q&A
   - Create/update FAQ pages in `wiki/faq/`
   - Update entity pages with misconceptions
   - Update `wiki/index.md`

5. **Commit & Push**

---

## Monthly Lint Workflow

Once a month (e.g., first Friday of each month):

1. **I run lint**: Check for broken links, stale claims, orphans, contradictions
2. **I report**: "Found 2 stale claims, 1 orphan page"
3. **You review**: Decide what to fix
4. **I update**: Fix confirmed issues
5. **Commit & Push**

---

## Image Placeholders Workflow

When you need an image in a wiki page:

### Example: Water balance flow diagram

1. **Create placeholder**:
   ```powershell
   # File: image_placeholders/Water_Balance_Flows_Fig_1_PLACEHOLDER.md
   ```

   **Content**:
   ```markdown
   # Image Placeholder: Water Balance Daily Flows

   **Location**: wiki/entities/Water_Balance.md  
   **Figure**: Fig 1  
   **Dimensions**: 1200 × 700 px  

   ## Legend / Description

   **Title**: Daily Water Balance Flows in APSIM

   **Subtitle**: Partitioning of rainfall into runoff, infiltration, 
   drainage, evaporation, and plant uptake. Typical summer day in 
   Mediterranean climate.

   ## Content

   [Detailed specification of diagram...]
   ```

2. **Reference in wiki page**:
   ```markdown
   See [[Image Placeholder: water_balance_flows_fig1_PLACEHOLDER.md]]
   ```

3. **Later**: When generating images, use placeholder as your brief

4. **Update wiki page**: Link to generated image (e.g., `image_placeholders/Water_Balance_Flows_Fig_1.png`)

5. **Mark placeholder**: "Complete"

---

## Quick Checklist

- [ ] Create GitHub repo (https://github.com/new)
- [ ] Replace `[USERNAME]` in git commands below with your GitHub username/org
- [ ] Run git commands to push local folder to GitHub
- [ ] Verify files visible on GitHub
- [ ] Read CLAUDE.md (at least overview section)
- [ ] Plan first source to ingest (e.g., APSIM_Internal_Guide)
- [ ] Share with team members (GitHub link)

---

## GitHub Commands (Copy-Paste Ready)

```powershell
# Navigate to wiki folder
cd "C:\Users\00113844\Documents\SCIE3314_APSIM_Tutorial\SCIE3314_APSIM_Wiki"

# Initialize Git
git init

# Stage all files
git add .

# Commit
git commit -m "Initial commit: LLM Wiki structure + bootstrap entities"

# Add GitHub remote (REPLACE [USERNAME] with your username/org)
git remote add origin https://github.com/[USERNAME]/SCIE3314_APSIM_Tutorial.git

# Set main branch and push
git branch -M main
git push -u origin main
```

---

## Questions?

- **Schema questions?** See CLAUDE.md
- **Page templates?** See CLAUDE.md (section "Page Template: Entity/Concept/FAQ")
- **Workflow questions?** See steps above or ask me during ingest

---

**Status**: ✅ Ready to push to GitHub

**Next Action**: Create GitHub repo + run git commands above
