# Marut_FCU — Repository Restructuring Agent Prompt

You are a repository restructuring agent for the Marut_FCU project.

## YOUR OBJECTIVE
Restructure the Marut_FCU GitHub repository into a clean domain-driven layout.
You must NOT modify the contents of any existing file — only move, rename,
and create new structural files (folders, .gitignore, empty stub .md files).

---

---

## STEP 1 — CLONE & AUDIT

1. Run the following and save the full output as `audit_before.txt`:
   ```
   find . -not -path './.git/*' | sort
   ```
2. Do not proceed until you have a complete picture of every file and folder present.

---

## STEP 2 — CATEGORISE BEFORE MOVING

Before moving anything, go through every file and folder in `audit_before.txt`
and assign each one to exactly one of these categories:

| Category   | Destination                          |
|------------|--------------------------------------|
| firmware   | firmware/core, nucleo, nf446re, modes/, debug/ |
| pcb        | pcb/                                 |
| composite  | composite/                           |
| frontend   | frontend/                            |
| meta       | .metadata/ (leave in place)          |
| root       | stays at root (README, .gitignore etc.) |
| unknown    | misc/                                |

A file is "unknown" if it does not clearly belong to any of the five domains
above and is not a standard root-level file. When in doubt, put it in misc/
rather than guessing — it is better to be honest about uncertainty than to
mis-file something. Print the full categorisation table before proceeding
so it can be reviewed.

---

## STEP 3 — RESTRUCTURE (moves only, no edits)

Reorganize files into the following target structure.
Use `git mv` for ALL moves so history is preserved.

```
Marut_FCU/
├── firmware/
│   ├── core/                  ← move MARUT_FCU here
│   ├── nucleo/                ← move MARUT_FCU_Nucleo here
│   ├── nf446re/               ← move MARUT_NF446RE here
│   ├── modes/
│   │   ├── rate/              ← move FCU_Quad_RateMode here
│   │   └── stabilize/        ← move FCU_Quad_StabilizeMode here
│   └── debug/                 ← move MCUV_Dbg.mcuvproj here
│
├── pcb/
│   ├── marut_ground_fill/     ← move Marut_GroundFILL here
│   │                            move MarutGNDFill.mcuvproj here
│   ├── schematics/            ← create empty folder (add .gitkeep)
│   ├── gerbers/               ← create empty folder (add .gitkeep)
│   └── bom/                   ← create empty folder (add .gitkeep)
│
├── composite/
│   ├── cad/                   ← create empty folder (add .gitkeep)
│   ├── layup_specs/           ← create empty folder (add .gitkeep)
│   └── simulations/           ← create empty folder (add .gitkeep)
│
├── frontend/                  ← move app/ contents here
│
├── misc/                      ← anything from the unknown category
│   └── README.md              ← create stub explaining this folder's purpose
│
├── docs/
│   ├── firmware/
│   │   ├── DOCUMENTATION.md   ← create stub
│   │   ├── PROGRESS.md        ← create stub
│   │   └── TASKS.md           ← create stub
│   ├── pcb/
│   │   ├── DOCUMENTATION.md   ← create stub
│   │   ├── PROGRESS.md        ← create stub
│   │   └── TASKS.md           ← create stub
│   ├── composite/
│   │   ├── DOCUMENTATION.md   ← create stub
│   │   ├── PROGRESS.md        ← create stub
│   │   └── TASKS.md           ← create stub
│   ├── frontend/
│   │   ├── DOCUMENTATION.md   ← create stub
│   │   ├── PROGRESS.md        ← create stub
│   │   └── TASKS.md           ← create stub
│   └── ARCHITECTURE.md        ← create stub
│
├── .metadata/                 ← leave in place, do not touch
├── .gitignore                 ← create or update (see Step 4)
├── README.md                  ← create stub if not present
└── CONTRIBUTING.md            ← create stub if not present
```

Rules:
- Use `git mv <src> <dst>` for every move. Never use `cp` or `mv`.
- If a target folder does not exist yet, create it with `mkdir -p` before the `git mv`.
- Do not open, edit, or overwrite any existing file's contents.
- Stub .md files must contain only a single heading line, e.g. `# Firmware Documentation` — nothing else.
- The misc/README.md stub should read: `# Misc — Files Pending Classification`

---

## STEP 4 — .gitignore

Create or update the root `.gitignore`. Apply the following rules:

EXCLUDE (add to .gitignore):
```
# OS noise
.DS_Store
Thumbs.db
desktop.ini

# Editor noise
.vscode/
.idea/
*.swp
*.swo
*~

# Python
__pycache__/
*.py[cod]
.env
.venv/

# Node
node_modules/
.npm/

# Logs
*.log
npm-debug.log*
```

DO NOT EXCLUDE — these must be explicitly absent from .gitignore
or have a negation rule if a prior rule would catch them:
- `*.bin`, `*.elf`, `*.hex`, `*.axf`  (firmware build outputs — keep tracked)
- `*.mcuvproj`                         (MCUVision project files — keep tracked)
- `build/`, `Build/`, `Debug/`, `Release/`  (build directories — keep tracked)
- `*.o`, `*.d`, `*.map`               (intermediate build artifacts — keep tracked)

If the repo already has a .gitignore that ignores any of the above,
add explicit negation lines at the bottom:
```
# Keep build outputs tracked
!*.bin
!*.elf
!*.hex
!*.axf
!*.mcuvproj
!build/
!Build/
!Debug/
!Release/
!*.o
!*.d
!*.map
```

Do not remove existing .gitignore rules that are not in conflict with the above.

---

## STEP 5 — COMMIT

Stage all changes and create a single commit:

```
git add -A
git commit -m "chore: restructure repo into domain-driven layout

- Moved firmware variants into firmware/core, nucleo, nf446re, modes/
- Moved PCB assets into pcb/
- Moved frontend app into frontend/
- Added misc/ for files pending classification
- Added docs/ stubs for Firmware, PCB, Composite, Frontend domains
- Updated .gitignore: exclude OS/editor noise, keep all build outputs tracked
- No file contents were modified"
```

Do NOT push yet.

---

## STEP 6 — VERIFY

After the commit, run `find . -not -path './.git/*' | sort` and save as
`audit_after.txt`.

Then verify ALL of the following checks and print a result table:

| #  | Check                                                                                          | Pass/Fail |
|----|-----------------------------------------------------------------------------------------------|-----------|
| 1  | firmware/core/ exists and contains the original MARUT_FCU files                               |           |
| 2  | firmware/nucleo/ exists and contains the original MARUT_FCU_Nucleo files                      |           |
| 3  | firmware/nf446re/ exists and contains the original MARUT_NF446RE files                        |           |
| 4  | firmware/modes/rate/ exists and contains the original FCU_Quad_RateMode files                 |           |
| 5  | firmware/modes/stabilize/ exists and contains the original FCU_Quad_StabilizeMode files       |           |
| 6  | firmware/debug/ contains MCUV_Dbg.mcuvproj                                                    |           |
| 7  | pcb/marut_ground_fill/ contains the original Marut_GroundFILL files and MarutGNDFill.mcuvproj |           |
| 8  | pcb/schematics/, pcb/gerbers/, pcb/bom/ exist                                                 |           |
| 9  | composite/cad/, composite/layup_specs/, composite/simulations/ exist                          |           |
| 10 | frontend/ contains the original app/ contents                                                 |           |
| 11 | misc/ exists                                                                                   |           |
| 12 | misc/README.md exists and contains only the stub heading                                       |           |
| 13 | Every file from the "unknown" category (Step 2 table) is present inside misc/                 |           |
| 14 | No "unknown" file remains at its original path                                                 |           |
| 15 | docs/firmware/, docs/pcb/, docs/composite/, docs/frontend/ each contain DOCUMENTATION.md, PROGRESS.md, TASKS.md |           |
| 16 | docs/ARCHITECTURE.md exists                                                                    |           |
| 17 | README.md exists at root                                                                       |           |
| 18 | CONTRIBUTING.md exists at root                                                                 |           |
| 19 | .gitignore does NOT ignore *.bin, *.elf, *.hex, *.axf                                         |           |
| 20 | .gitignore does NOT ignore *.mcuvproj                                                          |           |
| 21 | .gitignore does NOT ignore build/, Debug/, Release/                                            |           |
| 22 | No original file paths from audit_before.txt remain at their old locations (except .metadata/) |           |
| 23 | File count in audit_after.txt >= file count in audit_before.txt (no files deleted)            |           |
| 24 | git log --oneline -1 shows the chore: restructure commit                                      |           |
| 25 | Current branch is restructure/domain-layout, not main                                         |           |

If any check fails, fix the issue and re-run only that check before proceeding.

Once all 25 checks pass, print:

```
✅ Restructure complete. Ready for review and push.
```

Then show the final directory tree one more time using:
```
find . -not -path './.git/*' | sort
```
