# SMAX-Upgrade-Tool
A UI tool for SMAX upgrades with wizard-style stage navigation, markdown notes, and progress tracking
https://gianandr4.github.io/SMAX-Upgrade-Tool/

## File Structure

Each version folder (e.g., `24.4/`) contains:

### Supported File Formats

**Markdown Format (Recommended):**
- `client-D.md` - Markdown with YAML front-matter
- `client-E.md` - Better for documentation-heavy workflows
- Natural syntax for steps, notes, and commands

**YAML Format:**
- `client-A.yaml` - Traditional YAML structure
- `client-B.yaml` - Good for programmatic generation
- `client-C.yaml` - Compact structured format

### Legacy Files (Backward Compatible)
The tool automatically migrates old format files to the new unified structure:
- `client-X-managed-steps.yaml` / `client-X-embedded-steps.yaml` - Old step definitions
- `state-client-X.yaml` - Old state files (automatically merged on first load)

## Format Comparison

| Feature | Markdown (.md) | YAML (.yaml) |
|---------|----------------|--------------|
| **Readability** | ⭐⭐⭐⭐⭐ Excellent | ⭐⭐⭐ Good |
| **Editing Experience** | Natural, like writing docs | Structured, technical |
| **Git Diffs** | Clean, easy to review | More verbose |
| **Escape Issues** | None for markdown content | Quotes/special chars need escaping |
| **Stage Definition** | `# Stage Name` (H1 header) | `stages: - name: "Stage Name"` |
| **Step Definition** | `## Step Title` (H2 header) | `steps: - id: "step_id"` |
| **Commands** | ` ```bash ... ``` ` code blocks | `command: "..."` with escaping |
| **Completion Status** | `- [x]` or `- [ ]` checkboxes | `done: true/false` |
| **Personal Notes** | `**Personal Notes:** text` | `userNotes: "text"` |
| **Best For** | Documentation-heavy workflows | Programmatic generation |

## File Format Options

### Option 1: Markdown with Front-Matter (Recommended)

Markdown provides a more natural, readable format especially when dealing with rich documentation and notes.

**Structure:**
```markdown
---
config:
  namespace: "itsma-clienta"
  version: "24.4"
  registry_server: "registry.example.com"
  username: "admin"
---

# Preparation

## SMAX Health Check

Ensure all pods are running before starting.

```bash
kubectl get pods -n {{namespace}}
```

- [ ] Step not completed

**Personal Notes:**
Verified all pods healthy on 2024-01-15

---

## Create Docker Registry Secret

```bash
kubectl create secret docker-registry <image_secret_name> \
  --docker-username=<username> \
  --docker-****** \
  --docker-server=<registry_server> \
  -n <namespace>
```

- [x] Step completed

---

# Upgrade

## Apply Upgrade

```bash
./upgrade.sh -n {{namespace}} -v {{version}}
```

- [ ] Step not completed

---
```

**Benefits:**
- ✅ More readable and natural for documentation
- ✅ No YAML escaping issues for markdown content
- ✅ Better Git diffs and easier editing
- ✅ Familiar format (used in Jekyll, Hugo, etc.)
- ✅ Stages defined by H1 headers (`#`)
- ✅ Steps defined by H2 headers (`##`)
- ✅ Checkboxes indicate completion: `- [x]` or `- [ ]`
- ✅ Personal notes marked with `**Personal Notes:**`

### Option 2: YAML Structure

```yaml
config:
  namespace: "itsma-clienta"
  ESM_NAMESPACE: "itsma-clienta"
  version: "24.4"
  registry_server: "registry.example.com"
  username: "admin"
  password: "changeme"
  image_secret_name: "regcred"

stages:
  - name: "Preparation"
    steps:
      - id: "health_check"
        title: "SMAX Health Check"
        notes: "Ensure all pods are running before starting."
        command: "kubectl get pods -n {{namespace}}"
        done: false
        userNotes: ""
      
      - id: "create_secret"
        title: "Create Docker Registry Secret"
        notes: "Create secret for pulling images."
        command: "kubectl create secret docker-registry <image_secret_name> --docker-username=<username> --docker-password=<password> --docker-server=<registry_server> -n <ESM_NAMESPACE>"
        done: false
        userNotes: "**Completed**: Secret created successfully"
  
  - name: "Upgrade"
    steps:
      - id: "apply_upgrade"
        title: "Apply Suite Upgrade"
        command: "./upgrade.sh -n {{namespace}} -v {{version}}"
        done: false
        userNotes: ""
```

## Usage

1. **Deploy**: Host on GitHub Pages or any web server
2. **Open**: Navigate to the tool in your browser
3. **Authenticate**: Enter GitHub Personal Access Token (requires repo write access)
4. **Select**: Choose version and client from dropdowns
5. **Navigate**: Use stage tabs or Previous/Next buttons to navigate through stages
6. **Work**: Check off steps as completed, add personal notes in markdown
7. **Save**: Click "Save Progress" to persist all changes to GitHub

## Features

### Core Features
- ✅ **Stages Grouping**: Organize steps into logical stages (Preparation, Upgrade, Verification, etc.)
- ✅ **Wizard View**: Navigate through one stage at a time with Previous/Next buttons
- ✅ **Progress Tracking**: Visual progress bar showing completion across all stages
- ✅ **Stage Tabs**: Quick navigation between stages with completion indicators
- ✅ **Dual Format Support**: Choose between Markdown (.md) or YAML (.yaml) files
- ✅ **Unified Configuration**: Single file per client containing configuration, state, and notes

### Step Management
- ✅ **Checkbox Tracking**: Mark steps as done/undone with visual feedback
- ✅ **Command Interpolation**: Supports both `{{variable}}` and `<variable>` placeholder formats
- ✅ **Copy-to-Clipboard**: One-click copy for commands with variable substitution
- ✅ **Reset Functionality**: Reset all completed steps with confirmation dialog

### Markdown Notes
- ✅ **Per-Step Notes**: Add personal notes to each step in markdown format
- ✅ **Live Preview**: Real-time markdown rendering as you type
- ✅ **Rich Formatting**: Support for headings, lists, code blocks, bold, italic, etc.
- ✅ **Persistent Storage**: Notes saved back to file (YAML or Markdown)
- ✅ **Native Markdown Files**: Use .md files for better readability and editing experience

### File Format Support
- ✅ **Auto-Detection**: Automatically detects and loads .md or .yaml files
- ✅ **Markdown Front-Matter**: YAML configuration in front-matter, content in markdown
- ✅ **Bi-Directional Conversion**: Save changes back in original format
- ✅ **Format Choice**: Pick the format that works best for your workflow

### Backward Compatibility
- ✅ **Auto-Migration**: Automatically converts old format files to new unified structure
- ✅ **State Merging**: Merges old separate state files into unified format
- ✅ **Dynamic Client Loading**: Scans version folder for client files (.yaml or .md)

### Technical Features
- ✅ **GitHub API Integration**: Direct read/write to repository
- ✅ **Error Handling**: Comprehensive error messages and logging
- ✅ **Environment Variables**: Configurable per-client settings
- ✅ **XSS Protection**: Proper HTML escaping and sanitization

## Placeholder Formats

The tool supports two placeholder formats for command interpolation:

1. **Double Curly Braces**: `{{variable}}` - Traditional format
2. **Angle Brackets**: `<variable>` or `<VARIABLE>` - Common in kubectl commands

Example:
```bash
kubectl create secret docker-registry <image_secret_name> \
  --docker-username=<username> \
  --docker-password=<password> \
  --docker-server=<registry_server> \
  -n <ESM_NAMESPACE>
```

Both formats are replaced with actual values from the `config` section.

## Screenshots

![SMAX Upgrade Tool UI](https://github.com/user-attachments/assets/ee023815-19c0-44b4-a495-7853036521c0)

## Development

The tool is a single-page application built with:
- Vanilla JavaScript
- [js-yaml](https://github.com/nodeca/js-yaml) for YAML parsing
- [marked](https://marked.js.org/) for markdown rendering
- GitHub API for persistence

