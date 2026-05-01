# 🚀 SCIE3314 APSIM Wiki — Quick Reference Card

**Status**: ✅ Local setup complete  
**Location**: `C:\Users\00113844\Documents\SCIE3314_APSIM_Tutorial\SCIE3314_APSIM_Wiki\`

---

## 📋 What You Have

```
✅ CLAUDE.md               Schema + workflows + page templates (READ THIS)
✅ README.md              GitHub landing page
✅ SETUP_GUIDE.md         GitHub setup + first ingest walkthrough
✅ DELIVERY_SUMMARY.md    This delivery overview
✅ .gitignore             Git configuration

✅ wiki/index.md          Master catalog of all pages
✅ wiki/log.md            Ingest/query log
✅ wiki/entities/Wheat.md              ← Core APSIM concepts
✅ wiki/entities/ESW_Extractable_Soil_Water.md
✅ wiki/entities/Thermal_Time.md
✅ wiki/entities/Water_Balance.md

📁 raw/sources/           Drop APSIM docs/papers here
📁 raw/exercises/         Drop .apsimx files here
📁 raw/instructor_notes/  Drop Q&A logs here

📁 wiki/concepts/         Ready for pedagogic frameworks
📁 wiki/sources/          Ready for source summaries
📁 wiki/synthesis/        Ready for cross-cutting analysis
📁 wiki/faq/              Ready for troubleshooting
📁 wiki/exercises/        Ready for module/exercise guides

📁 image_placeholders/    Ready for diagram specs
```

---

## ⚡ Quick Start (3 Steps)

### Step 1: GitHub Setup (5 min)

```powershell
# Create repo at https://github.com/new
# Name: SCIE3314_APSIM_Tutorial
# Then run:

cd "C:\Users\00113844\Documents\SCIE3314_APSIM_Tutorial\SCIE3314_APSIM_Wiki"

git init
git add .
git commit -m "Initial commit: LLM Wiki structure + bootstrap entities"

# Replace [USERNAME] with your GitHub username/org
git remote add origin https://github.com/[USERNAME]/SCIE3314_APSIM_Tutorial.git
git branch -M main
git push -u origin main
```

### Step 2: First Source Ingest (15 min)

```powershell
# Copy a source to raw/sources/
cp "../REFERENCE_DOCS/APSIM_Internal_Guide.md" "raw/sources/"

# Tell me: "I've added APSIM_Internal_Guide to raw/sources/"
# I'll ingest, create entity pages, update index & log

# Then commit:
git add wiki/
git commit -m "Ingest: APSIM Internal Guide → entity pages"
git push
```

### Step 3: Start Teaching! (Ongoing)

- Add exercise `.apsimx` files to `raw/exercises/`
- Collect student Q&A in `raw/instructor_notes/`
- Review my updates in wiki/
- Commit & push monthly

---

## 📚 Key Documents (Read in Order)

| Order | Document | Time | Purpose |
|-------|----------|------|---------|
| 1️⃣ | SETUP_GUIDE.md | 10 min | GitHub setup + first steps |
| 2️⃣ | CLAUDE.md (Overview) | 10 min | Understand 3-layer architecture |
| 3️⃣ | README.md | 5 min | Project overview |
| 4️⃣ | CLAUDE.md (Full) | 30 min | Workflows + page templates |
| 5️⃣ | wiki/entities/Wheat.md | 10 min | Example: what an entity page looks like |

---

## 🔄 Three Core Workflows

### Workflow A: Ingest a Source
```
You: Add file to raw/sources/ (or raw/exercises/ or raw/instructor_notes/)
     ↓
Me:  Read → extract → create/update wiki pages → add to log
     ↓
You: Review → commit & push
```

### Workflow B: Answer a Question
```
You: Ask question or share student Q&A
     ↓
Me:  Search wiki + sources → synthesize answer → optionally create FAQ
     ↓
You: Review → optionally commit
```

### Workflow C: Monthly Lint
```
Me:  Check for broken links, stale claims, orphans, contradictions
     ↓
Me:  Report findings
     ↓
You: Decide what to fix
     ↓
Me:  Update pages → commit
```

---

## 🎯 Next Actions

**Today**:
- [ ] Read SETUP_GUIDE.md (5 min)
- [ ] Create GitHub repo
- [ ] Run git commands to push

**This Week**:
- [ ] Read CLAUDE.md Overview
- [ ] Add first source to raw/sources/
- [ ] Tell me to ingest

**This Month**:
- [ ] Ingest 2–3 sources
- [ ] Create exercise guides
- [ ] Collect first student Q&A

---

## 📞 Quick Help

**"How do I...?"**

| Question | Answer |
|----------|--------|
| ...set up GitHub? | See SETUP_GUIDE.md (git commands ready) |
| ...ingest a source? | Put in raw/sources/; tell me; I do the rest |
| ...add a new wiki page? | Use template in CLAUDE.md; wikilink from related pages |
| ...link between pages? | Use `[[PageName]]` (wikilinks) |
| ...update the catalog? | I auto-update wiki/index.md on each ingest |
| ...add an image? | Create placeholder in image_placeholders/; generate later |
| ...ask a question? | Share as Q&A log in raw/instructor_notes/ or directly |
| ...lint the wiki? | I do monthly; or ask me manually |

---

## 🎓 Understanding the Design

### Why LLM Wiki?
- **Knowledge compounds** (not re-derived on each query)
- **Cross-references built-in** (wikilinks, synthesis pages)
- **You curate sources** (decide what to ingest)
- **I maintain structure** (bookkeeping, links, consistency)
- **Scalable** (grows over time without degradation)

### Why Three Layers?
1. **Raw** (immutable) — Your source of truth
2. **Wiki** (synthesis) — LLM-generated connections
3. **Image specs** (placeholders) — Generate later

### Why Modules 0–7 + Exercises A–D?
- **Flexible** (teach 4 hrs or 8 hrs, 2 weeks or 4 weeks)
- **Modular** (each module stands alone or builds on others)
- **Scaffolded** (exercises progress from simple to complex)
- **Hands-on** (simulation-based, active learning)

### Why Near-Solved `.apsimx` Files?
- **Reduces cognitive load** (students focus on learning outcome, not setup)
- **Increases engagement** (results faster, more time for analysis)
- **Promotes understanding** (scaffolding → challenge → mastery)

---

## 📊 Current State

| Component | Status |
|-----------|--------|
| Folder structure | ✅ Complete |
| Schema (CLAUDE.md) | ✅ Complete (1,000+ lines) |
| Documentation | ✅ Complete (README, SETUP, DELIVERY) |
| Bootstrap entities | ✅ 4 pages (Wheat, ESW, Thermal_Time, Water_Balance) |
| Ingest workflow | ✅ Ready (documented in CLAUDE.md) |
| GitHub setup | ⏳ Ready (you create repo + push) |
| First ingest | ⏳ Ready (you add source + I ingest) |
| Teaching phase | ⏳ Ready (once wiki populated) |

---

## 📁 Folder Sizes (Approximate)

```
CLAUDE.md              ~70 KB (1,000+ lines; read once)
wiki/entities/         ~30 KB (4 entity pages; will grow)
wiki/concepts/         ~0 KB  (ready for your content)
wiki/synthesis/        ~0 KB  (ready for your content)
wiki/faq/              ~0 KB  (ready for Q&A)
wiki/exercises/        ~0 KB  (ready for guides)
raw/sources/           ~0 KB  (ready for docs)
raw/exercises/         ~0 KB  (ready for .apsimx files)
raw/instructor_notes/  ~0 KB  (ready for Q&A logs)
```

**Total**: ~100 KB (very lightweight; images will add volume later)

---

## 🚀 You're Ready!

Everything is set up locally and documented. Next step: push to GitHub and start ingesting sources.

---

**Questions?** See documents above or ask me during next conversation.

**Excited?** You should be — this is a powerful system for building and sharing knowledge! 🎉
