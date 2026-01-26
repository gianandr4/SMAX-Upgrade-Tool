# SMAX-Upgrade-Tool
A UI tool for SMAX upgrades streamlining and notes
https://gianandr4.github.io/SMAX-Upgrade-Tool/

## File Structure

Each version folder (e.g., `24.4/`) contains:

### Template Files
- `managed-steps.yaml` - Template for managed SMAX deployments
- `embedded-steps.yaml` - Template for embedded SMAX deployments

### Client-Specific Files
Each client has their own customized files:
- `client-A-managed-steps.yaml` - Client A's managed deployment steps
- `client-A-embedded-steps.yaml` - Client A's embedded deployment steps
- `state-client-A.yaml` - Client A's progress state

- `client-B-managed-steps.yaml` - Client B's managed deployment steps
- `client-B-embedded-steps.yaml` - Client B's embedded deployment steps
- `state-client-B.yaml` - Client B's progress state

- `client-C-managed-steps.yaml` - Client C's managed deployment steps
- `client-C-embedded-steps.yaml` - Client C's embedded deployment steps
- `state-client-C.yaml` - Client C's progress state

## Usage

1. Deploy to GitHub Pages or any web server
2. Open in browser
3. Enter GitHub Personal Access Token (requires repo write access)
4. Select version, deployment type (Managed/Embedded), and client
5. The tool will load the client-specific steps file
6. Check off steps as completed
7. Click "Save Progress" to persist state to GitHub
8. Use Copy buttons to copy commands to clipboard

## Features

- ✅ Hardcoded repository paths (no URL parsing issues)
- ✅ Copy-to-clipboard functionality for commands
- ✅ Step tracking with checkboxes and visual feedback
- ✅ State persistence via GitHub API
- ✅ Client-specific steps and state files
- ✅ Enhanced error handling and logging
- ✅ XSS protection with proper HTML escaping

