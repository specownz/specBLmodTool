# specBLmodTool v1.0 — User Manual

**by spec** · Mount & Blade II: Bannerlord Mod Creation Tool

---

## 1. Introduction

**specBLmodTool** is a Windows desktop suite for creating and maintaining Bannerlord modules with a focus on **XML-only content**, safety around core game files, packaging, and publishing helpers.

You can:

- Create and manage modules (`SubModule.xml`, folders)
- Edit structured XML with core-module protection
- Generate safe **XSLT** overrides
- Build **Items, Troops, Parties, Kingdoms, Clans, Cultures, Settlements**, and more in dedicated workbenches
- Save individual entities to a **personal library** and reuse them in other mods
- **Import** library/workbench content into the current working module (including `SubModule.xml` registration)
- Package mods as ZIP, draft Nexus/ModDB READMEs, validate load order, and more

---

## 2. Installation & requirements

1. Build or install **specBLmodTool** (.NET Framework 4.8).
2. Install **Mount & Blade II: Bannerlord** (Steam recommended).
3. Optional: **Modding Kit** (Steam Tools — app `1393600`) for the official editor.
4. Optional: **Git for Windows** for the Publish Hub Git tab.
5. Optional: Nexus personal API key for key validation in Publish Hub.

Launch the app and set:

- **Bannerlord Install** path (folder that contains `Modules`)
- **Modding Kit** path if you use *Launch Modding Kit*

Paths are remembered under `%AppData%\specBLmodTool\`.

---

## 3. Main window overview

| Area | Purpose |
|------|---------|
| Path fields | Game install + Modding Kit |
| Button grid | Quick access to major tools |
| Status bar | Ready state, game path, tool count, **current Module** |
| Menu strip | Full workflow (File, Tools, Module, Window, Help) |

### Recommended first steps

1. **File → Set Game Path…** (or Browse on the main form).
2. **File → New Module…** or open an existing one.
3. **File → Open Module from Game Modules…** to set the **working module**.
4. Use **Module** or **Tools → XML Entities** workbenches to add content.
5. **File → Package Module (ZIP)…** when ready to share.

---

## 4. Working module concept

Many actions need a **current module**:

- Status bar: `Module: YourModId`
- Set via:
  - **File → Open Module Folder…** (folder with `SubModule.xml`)
  - **File → Open Module from Game Modules…**
- Clear via **Module → Clear Current Module**

The working module is where **Import → Module** writes XML and updates `SubModule.xml`.

---

## 5. Menus

### File

| Command | Action |
|---------|--------|
| New Module | Scaffold module folders + `SubModule.xml` |
| Open Module Folder | Pick a module directory |
| Open Module from Game Modules | Choose from installed non-core modules |
| Open Module in Explorer | Windows Explorer at module root |
| Open ModuleData Folder | Explorer at `ModuleData` |
| Package Module (ZIP) | Vortex/Nexus-style `Modules/Id/...` zip |
| Export README | README / Nexus / ModDB text generator |
| Set Game / Kit Path | Configure installs |
| Exit | Close app (saves settings) |

### Tools

Grouped by category:

- **XML Entities** — Items, Troops, Parties, Kingdoms, Clans, Cultures, Settlements, Heroes, Equipment, Crafting, Body Properties, Workshops  
- **Content** — XML Mod Creator, XSLT, SubModule editor, Localization, New Module  
- **Assets** — Asset explorer, model library, 3D preview, BRF/TPAC  
- **Publishing** — README, Packager, Publish Hub  
- **System** — Config editors, load order, Modding Kit  

Opening a tool twice **activates the existing window** (single-instance).

### Module

Shortcuts for SubModule/XML/XSLT/Localization, common entity workbenches, load order, and **Quick Validate**.

### Window

List open tool windows; **Close All Tool Windows**.

### Help

Steam Modding Kit install, official/community docs, Nexus API docs, About.

---

## 6. XML Entity Workbenches (Items, Troops, …)

Each workbench is a full **create / view / edit** environment for one Bannerlord XML family.

### 6.1 Layout

| Panel | Use |
|-------|-----|
| **Personal Library** | Saved single entities (reusable across mods) |
| **Entities in document** | List of `id`s in the open XML document; filter box |
| **Attributes grid** | Edit attributes of the selected entity |
| **XML text** | Full entity or document XML (Consolas) |
| **Toolbar** | New, Add Entity, Library save/load/delete, Import → Module, Open/Save XML, Apply Attrs, Parse XML |

### 6.2 Efficient workflow

1. **New** — empty root document (`Items`, `NPCCharacters`, …).  
2. **Add Entity** — inserts a **starter template** with sensible defaults.  
3. Edit **id** and fields in the attribute grid → **Apply Attrs**.  
4. Or edit XML directly → **Parse XML** to merge back.  
5. **Save → Library** — stores the entity under `%AppData%\specBLmodTool\EntityLibrary\<type>\`.  
6. Later: **Load Library** (or double-click) to pull into the document.  
7. Set working module → **Import → Module**:
   - Merges into `ModuleData\<default_file>.xml` by `id` (replace if same id)
   - Registers `XmlName` in `SubModule.xml` with Campaign / CampaignStoryMode when missing  

### 6.3 Entity types

| Workbench | SubModule `XmlName` id | Default file | Root / child |
|-----------|------------------------|--------------|--------------|
| Items | Items | custom_items.xml | Items / Item |
| Troops / NPCCharacters | NPCCharacters | custom_troops.xml | NPCCharacters / NPCCharacter |
| Party Templates | partyTemplates | custom_party_templates.xml | partyTemplates / partyTemplate |
| Kingdoms | Kingdoms | custom_kingdoms.xml | Kingdoms / Kingdom |
| Clans / Factions | Factions | custom_clans.xml | Factions / Faction |
| Cultures | SPCultures | custom_cultures.xml | SPCultures / Culture |
| Settlements | Settlements | custom_settlements.xml | Settlements / Settlement |
| Heroes | Heroes | custom_heroes.xml | Heroes / Hero |
| Equipment Rosters | EquipmentRosters | custom_equipment_sets.xml | EquipmentRosters / EquipmentRoster |
| Crafting Pieces | CraftingPieces | custom_crafting_pieces.xml | CraftingPieces / CraftingPiece |
| Body Properties | BodyProperties | custom_body_properties.xml | BodyProperties / BodyProperty |
| Workshop Types | WorkshopTypes | custom_workshops.xml | WorkshopTypes / WorkshopType |

Templates are **starting points**. Always verify item/troop ids against game data (`SandBoxCore` / `SandBox`) and your load order.

### 6.4 Tips for troops & parties

- Troop `id` must be unique across all modules.  
- Equipment slots use `Item.<item_id>` references.  
- Party stacks use `troop="NPCCharacter.<troop_id>"`.  
- Import troops **before** party templates that reference them.  
- For lords: often need **NPCCharacter** + **Hero** + **Faction** coordination.

### 6.5 Tips for kingdoms & settlements

- Kingdoms may require `initial_home_settlement` (newer game versions).  
- Settlements need map/scene work beyond XML for full appearance; XML alone defines data bindings.  
- Prefer **new ids** rather than overwriting Native/SandBox ids unless you intend an override.

---

## 7. Other major tools

### Structured XML Mod Creator

Browse module XML files; attribute name/value editing; core modules lock attribute **names**. Prefer editing **your** module files.

### XSLT Generator

Build identity + delete/change/add transforms for safe overrides of earlier modules.

### SubModule.xml Editor

Name, Id, Version, SP/MP flags, dependency list, registered Xmls, raw XML.

### Localization Editor

Scan language/`*string*.xml` files; grid of id/text; save.

### Load Order & Dependencies

Lists modules and missing required/optional dependencies.

### Asset / Model / TPAC tools

Browse assets, list TPACs, preview OBJ meshes (export from TpacTool), launch external viewers.

### Config editors

`BannerlordConfig.txt` / `engine_config.txt` key-value grids (Documents / game config paths as detected).

### README generator

Auto-fill from module scan; Markdown / BBCode / plain / HTML; copy or save `README.md`.

### Publish Hub

Nexus API key validate + package ZIP; ModDB/browser helpers; Git init/status/commit/push.

### Mod Packager

ZIP with `Modules/<Id>/...` layout for Vortex/manual install.

---

## 8. Safety model

- **Core modules** (Native, SandBoxCore, SandBox, StoryMode, …): attribute **names** locked in structured editor; avoid writing core `SubModule.xml`.  
- **Backups**: writes often create backups via `SafetyGuard` before overwrite.  
- Prefer **new modules + merge/XSLT** over editing official files.  
- Always test in the Bannerlord launcher with a clean or backup save when changing world data.

---

## 9. Typical end-to-end scenarios

### A. New item pack

1. New Module → open it as working module.  
2. Tools → XML Entities → **Items Workbench**.  
3. Add Entity → set `id`, `name`, mesh, Type, stats → Apply Attrs.  
4. Save → Library (optional).  
5. Import → Module.  
6. Launch game with module enabled; check encyclopedia/inventory.

### B. Custom troop tree fragment

1. Troops Workbench → add several `NPCCharacter`s with upgrade targets as needed.  
2. Import → Module.  
3. Party Templates Workbench → stacks referencing `NPCCharacter.your_ids`.  
4. Import parties.  
5. Quick Validate module.

### C. Reuse last mod’s troops in a new mod

1. Open new module as working.  
2. Troops Workbench → Load Library entries → Import each (or paste multi-entity document and import all).

### D. Publish

1. README generator → Auto Generate → edit → Copy/Save.  
2. Package ZIP.  
3. Publish Hub → Nexus package / browser upload; optional Git commit.

---

## 10. Keyboard shortcuts (main form)

| Shortcut | Action |
|----------|--------|
| Ctrl+N | New Module |
| Ctrl+O | Open Module Folder |
| Ctrl+P | Package Module ZIP |
| Alt+F4 | Exit |

Tool windows are modeless; use **Window** menu to focus them.

---

## 11. Troubleshooting

| Issue | What to try |
|-------|-------------|
| Tools disabled | Set a valid Bannerlord install path |
| Import fails | Open a working module first |
| Entity missing in game | Check SubModule `Xmls` registration, load order, unique `id`, Campaign game type |
| Broken XML | Use Parse XML; fix well-formedness; restore backup |
| Core lock | By design for Native/SandBox* — put content in your module |
| Git buttons fail | Install Git and ensure `git` is on PATH |
| Nexus validate fails | Check API key at nexusmods.com → API Access |

Logs: `%AppData%\specBLmodTool\logs\`.

---

## 12. Credits

- **specBLmodTool v1.0** by **spec**  
- using **specology** framework  
- created with the help of **grok**  
- Bannerlord © TaleWorlds Entertainment  
- Community docs: [docs.bannerlordmodding.com](https://docs.bannerlordmodding.com/), [moddocs.bannerlord.com](https://moddocs.bannerlord.com/)

---

*End of user manual.*
EOF
echo manual written
# final brace check on new files
python3 - <<'PY'
from pathlib import Path
for p in Path('/home/workdir/artifacts/specBLmodTool/specBLmodTool').rglob('*.cs'):
    if 'bin' in p.parts or 'obj' in p.parts: continue
    t=p.read_text()
    if t.count('{')!=t.count('}'):
        print('MISMATCH', p, t.count('{'), t.count('}'))
print('done')
# verify EntityTypeCatalog in SpecApp
print('entity loop', 'EntityTypeCatalog' in Path('/home/workdir/artifacts/specBLmodTool/specBLmodTool/Spec/Core/SpecApp.cs').read_text())
PY
𝔭description_placeholder
Verify braces and write user manual

---

## 13. Mod templates & XML Mod Creation Wizard

### Quick create (New Module)

**File → New Module (Quick)** or the New Module Creator form:

1. Enter Id, Name, Version.
2. Choose a **Mod Template** (Empty, Items/Weapons, Troops, Troops+Parties, Kingdom/Faction, Culture, Settlements, Equipment, Crafting, Full XML starter).
3. **Create** scaffolds folders, `SubModule.xml` dependencies, empty ModuleData XML files, and `XmlName` registrations for the template types.
4. Or click **Open Full Wizard…**.

### Wizard (**File → New Module Wizard…** / Ctrl+Shift+N)

| Step | Purpose |
|------|---------|
| 1 Welcome | Choose template and read what it scaffolds |
| 2 Identity | Id, display name, version, SP/MP, set working module |
| 3 Dependencies | Check required modules (Native/SandBox* preselected) |
| 4 Content types | Fine-tune which entity XML types to include |
| 5 Library blocks | Check saved Entity Workbench entries to import |
| 6 Review | Summary → **Create Module** |

**Building-block workflow:** save troops/items/etc. in workbenches → Library → run wizard → check those entries → create. The new mod under `Modules\Id` receives merged XML and correct `SubModule.xml` registrations. The module can be set as the **working module** immediately for further imports.
