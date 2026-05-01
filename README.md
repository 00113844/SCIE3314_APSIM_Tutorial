# SCIE3314 APSIM Tutorial — LLM Wiki

A persistent, compounding knowledge base for APSIM crop modeling education at UWA.

**Status**: 🚀 Bootstrap phase (May 2026)

---

## About This Project

This wiki is designed to support **SCIE3314: Crops and Cropping Systems** at the University of Western Australia. It provides:

- **Structured knowledge**: Entity pages (Wheat, ESW, phenology), concepts (scaffolding, near-solved files), pedagogic framework
- **Exercise guides**: Detailed walkthroughs for 8 modules + integrated exercises A–D
- **Synthesis**: Cross-cutting analysis linking theory, simulation, and application
- **FAQ**: Student troubleshooting + common misconceptions
- **Long-term maintenance**: Knowledge compounds as sources are added and refined

### Three-Layer Architecture

1. **Raw sources** (`raw/`) — Immutable: APSIM docs, papers, instructor notes, `.apsimx` files
2. **Wiki** (`wiki/`) — LLM-generated synthesis: entities, concepts, sources, synthesis, FAQ, exercises
3. **Image placeholders** (`image_placeholders/`) — Specifications for diagrams (to be generated later)

See `CLAUDE.md` for full schema and workflows.

---

## Quick Start

### For Instructors

1. **Browse the wiki**: Start with `wiki/index.md`
2. **Add a source**: Drop a file into `raw/sources/`, `raw/exercises/`, or `raw/instructor_notes/`
3. **Ask a question**: Share student Q&A or teaching insights
4. **Review updates**: I'll summarize changes; you approve or redirect

### For Developers / Researchers

1. **Clone the repo**: `git clone https://github.com/[org]/SCIE3314_APSIM_Tutorial.git`
2. **Explore entities**: Read `wiki/entities/` for APSIM concepts
3. **Study synthesis**: See `wiki/synthesis/` for pedagogic frameworks
4. **Review exercises**: `wiki/exercises/` for detailed teaching guides

### For Students

1. **Read exercise guides**: `wiki/exercises/` for step-by-step instructions
2. **Troubleshoot**: Check `wiki/faq/` if issues arise
3. **Learn concepts**: `wiki/entities/` explains APSIM components

---

## File Structure

```
SCIE3314_APSIM_Wiki/
├── CLAUDE.md                  # Schema & workflows (read this first)
├── README.md                  # This file
├── .gitignore                 # Git ignore rules
│
├── raw/                       # Layer 1: Immutable sources
│   ├── sources/               # APSIM docs, papers, pedagogic guides
│   ├── exercises/             # .apsimx files + design notes
│   └── instructor_notes/      # Classroom observations, Q&A logs
│
├── wiki/                      # Layer 2: LLM-generated synthesis
│   ├── index.md              # Master catalog (auto-updated)
│   ├── log.md                # Ingest & query log
│   ├── entities/             # APSIM components: Wheat, ESW, Manager, etc.
│   ├── concepts/             # Pedagogic ideas: Scaffolding, Near-Solved Files, etc.
│   ├── sources/              # Summaries of ingested sources
│   ├── synthesis/            # Cross-cutting analysis
│   ├── faq/                  # Troubleshooting & student Q&A
│   └── exercises/            # Detailed exercise guides (Modules 0–7, Exercises A–D)
│
└── image_placeholders/        # Layer 3: Image specifications (for future generation)
    ├── [Page_Name]_Fig_[N]_PLACEHOLDER.md
    └── ...
```

---

## Key Workflows

### Ingest a Source

1. Add file to `raw/sources/` (or `raw/exercises/`, `raw/instructor_notes/`)
2. I read and summarize key takeaways
3. I create/update wiki pages (entities, concepts, source summary)
4. I add entry to `wiki/log.md`
5. You review; approve or provide feedback

**Example**: Add `Zadok_Scale_Reference.md` to `raw/sources/` → I create [[Zadok_Scale.md]], update [[Phenology.md]], link to [[Phenology_Sowing_Connection.md]]

### Answer a Question

1. You ask a question or share student Q&A
2. I search wiki pages + sources
3. I synthesize answer with citations
4. I optionally file answer as FAQ or synthesis page
5. I add entry to `wiki/log.md`

### Monthly Lint

1. I check for: broken links, stale claims, orphan pages, contradictions, incomplete placeholders
2. I report findings
3. You decide what to fix
4. I update pages

---

## Conventions

- **Wikilinks**: Use `[[PageName]]` for cross-references
- **Frontmatter**: Every page has YAML metadata (title, tags, related pages)
- **Citations**: Every claim cites a source
- **Page names**: CamelCase or Underscored_Case (no spaces)
- **Image references**: Use placeholder markdown specs; images generated later
- **No videos**: All pedagogic content is text-based wiki pages + exercise `.apsimx` files

---

## Contributing

### Adding a Source

1. **Prepare**: Write or collect your source (APSIM guide, research paper, instructor notes, exercise file)
2. **Place**: Add to `raw/sources/`, `raw/exercises/`, or `raw/instructor_notes/`
3. **Notify**: Tell me you've added a source
4. **Review**: I'll ingest it and show you updates
5. **Approve**: Review and confirm or redirect

### Creating an Exercise Guide

Use the template in `CLAUDE.md` (section "Page Template: Exercise"). I'll help you structure it once you have:
- Learning outcomes
- Associated `.apsimx` file (in `raw/exercises/`)
- Student instructions
- Worked answer

### Image Specifications

When a wiki page needs an image:
1. Create a placeholder file in `image_placeholders/`
2. Include detailed legend + visual specification
3. Later, generate image based on placeholder spec
4. Update wiki page with image link
5. Mark placeholder as complete

---

## Status & Roadmap

### ✅ Completed
- Schema design (`CLAUDE.md`)
- Folder structure
- Documentation

### 🚀 In Progress
- Bootstrap entity pages from APSIM reference docs
- Create initial concept pages (Scaffolding, Near-Solved Design)
- Generate exercise guides for Modules 0–7 + Exercises A–D

### 📋 Upcoming
- Ingest APSIM Next Gen official tutorials
- Ingest research papers (phenology, N response, water balance)
- Collect student Q&A from first teaching cycle
- Generate image placeholders → actual images

---

## Resources

### APSIM Documentation
- [APSIM Initiative](https://www.apsim.info/)
- [APSIM Next Gen Tutorials](https://apsimnextgeneration.netlify.app/user_tutorials/)
- [APSIM Next Gen Community](https://www.apsim.info/)

### Pedagogic Framework
- Bloom's Taxonomy (scaffolding progression)
- Situated Learning (authentic tasks with near-solved examples)
- Constructivism (students build understanding through simulation)

---

## License

MIT License — See LICENSE file

---

## Maintainers

- **Instructor**: [Your name]
- **LLM Agent**: Claude (Copilot chat interface)
- **Contributors**: UWA SCIE3314 teaching team

---

## Questions?

Refer to `CLAUDE.md` for detailed schemas, workflows, and templates.

For specific questions about APSIM, see:
- `wiki/entities/` for component explanations
- `wiki/faq/` for troubleshooting
- `wiki/exercises/` for teaching guides

---

**Last Updated**: May 2026  
**Wiki Version**: 1.0  
**Next Review**: June 2026
