# SMAX-Upgrade-Tool
A comprehensive UI tool for SMAX upgrades with wizard-style navigation, inline editing, markdown notes, dark mode, and GitHub integration.

🔗 **Live Demo:** https://gianandr4.github.io/SMAX-Upgrade-Tool/

## ✨ New Features (Latest Update)

### 🎨 Enhanced UI
- **Dark Mode** - Toggle between light and dark themes (persistent via localStorage)
- **Inline Editing** - Click to edit titles, stages, steps, commands, and descriptions
- **Minimal Notes UI** - Compact 💬 button per step to expand/collapse note editor
- **Autosave** - Optional auto-save with 3-second debounce
- **Raw Editor** - Edit entire file content with syntax highlighting

### 🚀 Advanced Features
- **Image Upload** - Paste images (Ctrl+V) or drag-drop directly into notes, auto-uploads to GitHub
- **Add Steps/Stages** - Buttons to dynamically add new content
- **Version Upgrade** - Clone configuration to new version with automatic history archiving
- **YAML Migration** - Automatic one-time conversion of YAML files to Markdown

## File Structure

Version folders are organized under the `SMAX/` directory (e.g., `SMAX/24.4/`) and contain Markdown files with YAML front-matter.

### Directory Structure
```
SMAX/
├── 24.4/
│   ├── client-D.md
│   ├── client-E.md
│   └── images/
├── 24.5/
│   ├── client-D.md
│   └── images/
└── 24.6/
    └── client-X.md
```

### Markdown Format Structure

The tool uses a three-level hierarchy with clear step boundaries:
- **# (H1)** - Document title (e.g., "Client A Upgrade to v24.4")
- **## (H2)** - Stages (e.g., "Preparation", "Upgrade", "Verification")
- **### (H3)** - Steps (e.g., "Health Check", "Create Secret", etc.)
- **### - [x] Complete** - Completed step marker
- **### - [ ] Complete** - Incomplete step marker

Example:
```markdown
---
config:
  namespace: itsma-clienta
  version: '24.4'
---

# Client A Upgrade to v24.4

## Preparation

### SMAX Health Check

Ensure all pods are running.

\`\`\`bash
kubectl get pods -n {{namespace}}
\`\`\`

#### Notes

Current version notes here.

##### 24.3
Previous version notes here.

### - [ ] Complete
```

### Legacy Files (Auto-Migrated)
Old YAML format files are automatically converted to Markdown on first load:
- `client-X.yaml` - Converted to `client-X.md`
- `client-X-managed-steps.yaml` / `client-X-embedded-steps.yaml` - Old step definitions
- `state-client-X.yaml` - Old state files (automatically merged)

## Usage

1. **Open Tool**: Navigate to https://gianandr4.github.io/SMAX-Upgrade-Tool/ or host locally
2. **Authenticate**: Enter GitHub Personal Access Token (requires repo write access)
3. **Select**: Choose version and client from dropdowns
4. **Navigate**: Use stage tabs or Previous/Next buttons
5. **Edit Inline**: Click any title, stage, step, command, or description to edit
6. **Add Notes**: Click 💬 button on any step to add/edit markdown notes
7. **Upload Images**: Paste (Ctrl+V) or drag images into note editor
8. **Export PDF**: Click "Export PDF" to generate a PDF of the current stage
9. **Toggle Autosave**: Enable optional auto-save for automatic GitHub syncing
10. **Save**: Click "Save Progress" to persist changes to GitHub

### Keyboard Shortcuts & Interactions
- **Click** - Edit any text element (title, stage, step, command, description)
- **Double-click** - Edit stage names in tabs
- **Ctrl+V** - Paste images into note editor
- **Drag & Drop** - Drop images into note editor

## Features

### Core Features
- ✅ **Three-Level Hierarchy**: Document title → Stages → Steps
- ✅ **Wizard View**: Navigate one stage at a time with Previous/Next buttons
- ✅ **Progress Tracking**: Visual progress bar showing completion across all stages
- ✅ **Stage Tabs**: Quick navigation with completion indicators
- ✅ **Markdown-Only**: Simplified format, YAML auto-migrated on load

### Editing Features
- ✅ **Inline Editing**: Click to edit any content element
- ✅ **Add Steps/Stages**: Dynamic content creation with dedicated buttons
- ✅ **Raw Editor**: Full file editing with markdown syntax
- ✅ **PDF Export**: Export current stage to PDF for sharing/printing
- ✅ **Command Interpolation**: Supports both `{{variable}}` and `<variable>` formats
- ✅ **Copy-to-Clipboard**: One-click copy for commands with variable substitution

### Notes & Documentation
- ✅ **Minimal Notes UI**: 💬 button per step (shows when notes exist)
- ✅ **Rich Markdown**: Full markdown support with live preview
- ✅ **Image Upload**: Direct paste/drag-drop with GitHub integration
- ✅ **Persistent Storage**: All changes saved to GitHub

### Advanced Features
- ✅ **Dark Mode**: Toggle with theme persistence
- ✅ **Autosave**: Optional 3-second debounced auto-save
- ✅ **Version Upgrade**: Clone to new version with history archiving
- ✅ **Backward Compatibility**: Auto-migration from YAML format
- ✅ **Reset Functionality**: Reset all completion states with confirmation

### Technical Features
- ✅ **GitHub API Integration**: Direct read/write to repository
- ✅ **Error Handling**: Comprehensive error messages and logging
- ✅ **Environment Variables**: Configurable per-client settings
- ✅ **XSS Protection**: Proper HTML escaping and sanitization
- ✅ **Image Upload API**: Direct image upload to GitHub repository

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

### Light Mode
![SMAX Upgrade Tool Light Mode](https://github.com/user-attachments/assets/283c9304-313f-43db-812b-07bc64dab89e)

### Dark Mode
![SMAX Upgrade Tool Dark Mode](https://github.com/user-attachments/assets/8b188efc-cfb3-469d-b124-ff173c92c840)

## Development

The tool is a single-page application built with:
- Vanilla JavaScript
- [js-yaml](https://github.com/nodeca/js-yaml) for YAML parsing (front-matter only)
- [marked](https://marked.js.org/) for markdown rendering
- GitHub API for persistence and image uploads

### Local Development
```bash
# Serve locally
python3 -m http.server 8080
# or
npx serve .
```

Then navigate to `http://localhost:8080`

## Migration Guide

### From YAML to Markdown
When you first load a YAML file after this update:
1. The tool automatically converts it to Markdown format
2. All data, state, and notes are preserved
3. The new `.md` file is created in the same directory
4. Original YAML file remains unchanged

### Manual Conversion
If you prefer to convert manually:
1. Click "Raw Editor" button
2. Copy the displayed markdown
3. Save to a new `.md` file
4. Update file references

## Contributing

Contributions welcome! The tool uses a simple, maintainable architecture:
- Single `index.html` file for easy deployment
- No build process required
- Clean separation between UI and data logic

## License

MIT License - See LICENSE file for details

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
