# SMAX-Upgrade-Tool
A UI tool for SMAX upgrades with wizard-style stage navigation, markdown notes, and progress tracking
https://gianandr4.github.io/SMAX-Upgrade-Tool/

## File Structure

Each version folder (e.g., `24.4/`) contains markdown files for each client.

### File Format

**Markdown Format (Required):**
- `client-D.md` - Example client D configuration
- `client-E.md` - Example client E configuration with comprehensive stages
- `template.md` - Comprehensive template for new clients

All files use markdown with YAML front-matter.

## File Format

Markdown provides a natural, readable format for documentation-heavy workflows with excellent Git diffs.

**Structure:**
```markdown
---
config:
  namespace: "itsma-clienta"
  version: "24.4"
  registry_server: "registry.example.com"
  username: "admin"
---

# CLIENT A - SMAX Upgrade 24.4 → 24.6

## Preparation

### SMAX Health Check

Ensure all pods are running before starting.

```bash
kubectl get pods -n {{namespace}}
```

- [ ] Step not completed

**Personal Notes:**
Verified all pods healthy on 2024-01-15

---

### Create Docker Registry Secret

```bash
kubectl create secret docker-registry <image_secret_name> \
  --docker-username=<username> \
  --docker-password=<password> \
  --docker-server=<registry_server> \
  -n <namespace>
```

- [x] Step completed

---

## Upgrade

### Apply Upgrade

```bash
./upgrade.sh -n {{namespace}} -v {{version}}
```

- [ ] Step not completed

---
```

**Structure Details:**
- **H1 (#)**: Document title (e.g., "CLIENT A - SMAX Upgrade 24.4 → 24.6")
- **H2 (##)**: Stage name (e.g., "Preparation", "Upgrade", "Verification")
- **H3 (###)**: Step title (e.g., "SMAX Health Check", "Apply Upgrade")
- **Code blocks**: Commands with `{{variable}}` or `<variable>` placeholders
- **Checkboxes**: `- [x]` for completed, `- [ ]` for incomplete
- **Personal Notes**: Marked with `**Personal Notes:**` heading

**Benefits:**
- ✅ More readable and natural for documentation
- ✅ No escaping issues for markdown content
- ✅ Better Git diffs and easier editing
- ✅ Familiar format (used in Jekyll, Hugo, etc.)
- ✅ Clear hierarchy: Title > Stage > Step

## Usage

1. **Deploy**: Host on GitHub Pages or any web server
2. **Open**: Navigate to the tool in your browser
3. **Authenticate**: Enter GitHub Personal Access Token (requires repo write access)
4. **Select**: Choose version and client from dropdowns (dynamically loaded from repository)
5. **Navigate**: Use stage tabs or Previous/Next buttons to navigate through stages
6. **Work**: Check off steps as completed, add personal notes in markdown
7. **Save**: Click "Save Progress" to persist all changes to GitHub

## Features

### Core Features
- ✅ **Stages Grouping**: Organize steps into logical stages (Preparation, Upgrade, Verification, etc.)
- ✅ **Wizard View**: Navigate through one stage at a time with Previous/Next buttons
- ✅ **Progress Tracking**: Visual progress bar showing completion across all stages
- ✅ **Stage Tabs**: Quick navigation between stages with completion indicators
- ✅ **Markdown Format**: Single format for all client files
- ✅ **Unified Configuration**: Single file per client containing configuration, state, and notes
- ✅ **Dynamic Loading**: Version folders and client files are scanned automatically

### Step Management
- ✅ **Checkbox Tracking**: Mark steps as done/undone with visual feedback
- ✅ **Command Interpolation**: Supports both `{{variable}}` and `<variable>` placeholder formats
- ✅ **Copy-to-Clipboard**: One-click copy for commands with variable substitution
- ✅ **Reset Functionality**: Reset all completed steps with confirmation dialog

### Markdown Notes
- ✅ **Per-Step Notes**: Add personal notes to each step in markdown format
- ✅ **Live Preview**: Real-time markdown rendering as you type
- ✅ **Rich Formatting**: Support for headings, lists, code blocks, bold, italic, etc.
- ✅ **Persistent Storage**: Notes saved back to markdown file

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

## Creating a New Client

1. Copy `template.md` to `24.4/client-new.md` (or appropriate version folder)
2. Update the YAML front-matter with client-specific configuration
3. Update the H1 title with client name and version
4. Customize stages and steps as needed
5. Commit and push to repository
6. Refresh the tool and select the new client

## Screenshots

![SMAX Upgrade Tool UI](https://github.com/user-attachments/assets/ee023815-19c0-44b4-a495-7853036521c0)

## Development

The tool is a single-page application built with:
- Vanilla JavaScript
- [js-yaml](https://github.com/nodeca/js-yaml) for YAML parsing
- [marked](https://marked.js.org/) for markdown rendering
- GitHub API for persistence

## GitHub Token Permissions

Required permissions for GitHub Personal Access Token:
- `repo` (Full control of private repositories)

## Security

- Token is stored only in browser memory (not persisted)
- All API calls use HTTPS
- Markdown content is sanitized before rendering
- XSS protection in place

## Browser Support

- Chrome/Edge (latest)
- Firefox (latest)
- Safari (latest)

## License

MIT License - See repository for details
