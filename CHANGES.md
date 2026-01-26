# SMAX Upgrade Tool - Enhancement Summary

## Overview
This PR transforms the SMAX Upgrade Tool from a simple step list into a comprehensive wizard-based upgrade workflow tool with dual file format support.

## Major Features Added

### 1. Stage-Based Workflow
- Steps are now organized into named stages (e.g., "Preparation", "Upgrade", "Verification")
- Wizard-style navigation: one stage at a time with Previous/Next buttons
- Stage tabs for quick navigation between stages
- Progress bar showing overall completion across all stages
- Each stage tab displays completion status (e.g., "3/5 steps")

### 2. Markdown File Format Support
**Why Markdown?** Markdown provides a more natural, readable format for documentation-heavy workflows, eliminating YAML escaping issues and improving Git diffs.

**Format Structure:**
```markdown
---
config:
  namespace: "itsma-clienta"
  version: "24.4"
---

# Preparation

## Health Check
Description here
\`\`\`bash
kubectl get pods -n {{namespace}}
\`\`\`
- [x] Step completed
**Personal Notes:** Additional notes here
```

**Key Features:**
- H1 headers (`#`) define stages
- H2 headers (`##`) define steps
- Code blocks contain commands
- Checkboxes (`- [x]` / `- [ ]`) track completion
- Personal notes section for user-added content

### 3. Unified Client Configuration
- Single file per client containing everything: config vars, stages, steps, state, notes
- Eliminates separate state files
- Supports both `.yaml` and `.md` formats
- Auto-detects format based on file extension

### 4. Markdown Notes per Step
- Add personal notes to any step in markdown format
- Live preview as you type
- Full markdown support: headings, lists, code blocks, emphasis, etc.
- Notes persist to file (YAML or Markdown)

### 5. Enhanced Command Interpolation
Supports two placeholder formats:
- `{{variable}}` - Traditional curly brace format
- `<variable>` - Angle bracket format (common in kubectl commands)
- Case-insensitive matching for angle brackets

Example:
```bash
kubectl create secret docker-registry <image_secret_name> \
  --docker-username=<username> \
  --docker-****** \
  --docker-server=<registry_server> \
  -n <ESM_NAMESPACE>
```

### 6. Reset Functionality
- "Reset All Done Steps" button clears all completion states
- Confirmation modal prevents accidental resets
- Updates both UI and persistent storage

### 7. Backward Compatibility
- Automatically migrates old format files on first load
- Merges separate state files into unified configuration
- Supports legacy file structure during transition

## File Format Comparison

| Aspect | Markdown | YAML |
|--------|----------|------|
| Readability | Excellent | Good |
| Editing | Natural | Technical |
| Git Diffs | Clean | Verbose |
| Escaping | None needed | Required for special chars |
| Best For | Documentation-heavy | Programmatic generation |

## UI Changes

### Removed:
- `selType` dropdown (managed/embedded distinction no longer needed)

### Added:
- Stage tabs with completion indicators
- Progress bar and percentage
- Stage navigation buttons (Previous/Next)
- Reset button with modal confirmation
- Per-step markdown note editor with live preview

### Enhanced:
- Client selector now dynamically scans for both `.yaml` and `.md` files
- Visual feedback for completed steps
- Better error handling and status messages

## Technical Implementation

### New Functions:
- `parseMarkdownFrontMatter()` - Extracts YAML front-matter from markdown
- `parseMarkdownStages()` - Parses markdown content into stages/steps
- `tryLoadMarkdown()` - Attempts to load markdown format file
- `convertToMarkdown()` - Converts internal structure back to markdown
- `renderStageTabs()` - Renders stage navigation tabs
- `renderCurrentStage()` - Displays current stage in wizard view
- `updateProgress()` - Updates progress bar
- `prevStage()` / `nextStage()` - Stage navigation
- `toggleNoteEditor()` / `saveNote()` - Note editing functionality

### Updated Functions:
- `fetchClients()` - Now scans for both `.yaml` and `.md` files
- `loadData()` - Tries markdown first, falls back to YAML
- `saveData()` - Saves in original format (markdown or YAML)
- `interpolate()` - Supports both `{{var}}` and `<var>` formats

## Sample Files

### Markdown Format:
- `24.4/client-D.md` - Basic markdown example
- `24.4/client-E.md` - Rich example with multiple stages and detailed notes

### YAML Format:
- `24.4/client-A.yaml` - Simple upgrade workflow
- `24.4/client-B.yaml` - Multi-stage workflow with some completed steps
- `24.4/client-C.yaml` - Minimal configuration

## Migration Path

For existing users:
1. Old format files (`client-X-managed-steps.yaml` + `state-client-X.yaml`) continue to work
2. On first load, they're automatically migrated to new unified format
3. Users can then choose to convert to markdown format for better readability

## Benefits

1. **Better Documentation**: Markdown format makes it easy to write rich documentation
2. **Improved Organization**: Stages group related steps logically
3. **Better UX**: Wizard-style navigation focuses attention on current stage
4. **Flexibility**: Choose format that suits your workflow (markdown or YAML)
5. **Progress Tracking**: Visual indicators show completion status at a glance
6. **No Lock-In**: Switch between formats or continue using legacy format

## Testing

The implementation includes:
- Sample files in both formats
- Backward compatibility with legacy files
- Error handling for malformed files
- Console logging for debugging

## Future Enhancements

Possible additions:
- Export to PDF/HTML
- Stage-specific reset (instead of all steps)
- Step dependencies (block later steps until prerequisites complete)
- Multi-user collaboration features
- Step timing/duration tracking
