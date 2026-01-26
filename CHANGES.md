# SMAX Upgrade Tool - Enhancement Summary

## Overview
This update refines the SMAX Upgrade Tool to use a simplified markdown-only format with an improved hierarchical structure (H1=Title, H2=Stage, H3=Step).

## Major Changes

### 1. Markdown Structure Update
**New Hierarchy:**
- **H1 (#)**: Document title (e.g., "CLIENT A - SMAX Upgrade 24.4 → 24.6")
- **H2 (##)**: Stage name (e.g., "Preparation", "Binaries Download", "Upgrade")
- **H3 (###)**: Step title (e.g., "SMAX Health Check", "Apply Upgrade")

**Benefits:**
- Clearer document organization with explicit title
- Better semantic hierarchy
- Improved readability
- Consistent with standard markdown documentation practices

### 2. Markdown-Only Format
**Removed:**
- All YAML file support (.yaml files)
- Legacy file migration logic
- Dual format handling complexity

**Why Markdown Only?**
- Simpler codebase (no format branching)
- Better Git diffs and editing experience
- More natural for documentation-heavy workflows
- No YAML escaping issues

### 3. Dynamic Version and Client Loading
**Improvements:**
- Version dropdown populated dynamically from repository folders
- No hardcoded version options
- Client dropdown updates automatically when version changes
- Added `onVersionChange()` handler

**Benefits:**
- Works with any version folder structure
- No code changes needed to add new versions
- Automatic discovery of client files

### 4. Comprehensive Template
**New File:** `template.md`
- 9 major stages covering full upgrade lifecycle
- 40+ detailed steps with commands and notes
- Covers: Planning, Binaries, Backup, Validation, Registry, Upgrade, Post-Upgrade, Testing, Optimization, Finalization
- Ready to copy and customize for new clients

### 5. File Cleanup
**Removed all legacy files:**
- `*.yaml` - Old YAML format files
- `*-managed-steps.yaml` - Legacy managed format
- `*-embedded-steps.yaml` - Legacy embedded format
- `state-*.yaml` - Old state files
- `state-*.json` - Old state files

**Kept:**
- `client-D.md` - Updated to new H1/H2/H3 structure
- `client-E.md` - Updated to new H1/H2/H3 structure with comprehensive example
- `template.md` - New comprehensive template

## UI Features (Unchanged)

### Core Features
- ✅ **Stages Grouping**: Organize steps into logical stages
- ✅ **Wizard View**: Navigate through one stage at a time
- ✅ **Progress Tracking**: Visual progress bar across all stages
- ✅ **Stage Tabs**: Quick navigation with completion indicators

### Step Management
- ✅ **Checkbox Tracking**: Mark steps as done/undone
- ✅ **Command Interpolation**: Supports `{{variable}}` and `<variable>` formats
- ✅ **Copy-to-Clipboard**: One-click copy for commands
- ✅ **Reset Functionality**: Reset all steps with confirmation

### Markdown Notes
- ✅ **Per-Step Notes**: Add personal notes in markdown
- ✅ **Live Preview**: Real-time markdown rendering
- ✅ **Rich Formatting**: Full markdown support
- ✅ **Persistent Storage**: Saves back to GitHub

## Migration Guide

For users with existing YAML files:

1. **Manual Conversion:** Use the new `template.md` as a reference
2. **Structure:** Convert to H1=Title, H2=Stage, H3=Step format
3. **Config:** Move config section to YAML front-matter
4. **Steps:** Convert each step to H3 with description, command, checkbox, and notes
5. **Save:** Commit the new .md file to your version folder

## File Format Example

```markdown
---
config:
  namespace: "itsma-client"
  version: "24.6"
  registry_server: "registry.example.com"
---

# CLIENT A - SMAX Upgrade 24.4 → 24.6

## Preparation

### SMAX Health Check

Description of the step.

```bash
kubectl get pods -n {{namespace}}
```

- [ ] Step not completed

**Personal Notes:**
Add your notes here

---

### Next Step

...
```

## Technical Implementation

### Updated Functions
- `parseMarkdownStages()` - Now parses H1 as title, H2 as stage, H3 as step
- `convertToMarkdown()` - Generates new structure with optional title parameter
- `fetchClients()` - Only scans for .md files
- `loadData()` - Simplified to markdown-only
- `saveData()` - Always saves as markdown
- `onVersionChange()` - New handler for version dropdown

### Removed Functions
- `migrateFromOldFormat()` - No longer needed

### Code Simplification
- Removed ~150 lines of YAML handling code
- Removed dual-format branching logic
- Cleaner, more maintainable codebase

## Breaking Changes

⚠️ **YAML files are no longer supported**
- All client configurations must be in .md format
- Old .yaml files will not load
- Migration must be done manually using template.md as reference

## Screenshots

### Before (Dynamic Version Loading)
![Before](https://github.com/user-attachments/assets/44b84a74-c1e2-4c5c-bc06-09c05cc61fb6)

### After (Dynamic Version & Markdown-Only)
![After](https://github.com/user-attachments/assets/0b1b4ced-2ee7-47e7-ada6-2a7989122430)

## Benefits Summary

1. **Simpler**: Single format, less code complexity
2. **Clearer**: Better document hierarchy with explicit titles
3. **Easier**: More natural editing experience
4. **Maintainable**: Cleaner codebase with less branching
5. **Flexible**: Dynamic version/client discovery
6. **Comprehensive**: Rich template with 40+ steps
7. **Consistent**: Standard markdown practices

## Future Enhancements

Possible additions:
- Export to PDF/HTML
- Step dependencies
- Multi-user collaboration
- Duration tracking
- Approval workflows
