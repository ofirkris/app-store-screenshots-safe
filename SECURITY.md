# Security Policy

## Scope

This repository contains a **skill** (prompt-based instruction set) for AI coding agents. It does not contain executable code — it instructs agents to generate and execute code. Security concerns are therefore about:

1. **Prompt injection** — user inputs that cause the agent to perform unintended actions
2. **Supply chain** — dependencies pulled from npm during scaffolding
3. **Code injection** — user-provided values (colors, fonts, text) embedded in generated code
4. **Path traversal** — user-provided file paths accessing files outside the project

## Reporting a Vulnerability

If you find a security issue, please open a GitHub issue on this repository.

## Hardening Measures

### Input Validation
- **Colors**: Validated against CSS color formats (hex, named, rgb/hsl, gradients); blocks `url()`, `expression()`, `eval()`, `javascript:`, and other injection vectors
- **Font names**: Alphanumeric, spaces, and hyphens only (max 60 chars)
- **File paths**: Must be relative, no `..`, no null bytes, restricted to image extensions
- **Text content**: No HTML tags (except `<br />`), no `javascript:` URIs, no template literals
- **Brand preset files**: All values validated with the same rules as interactive input

### Dependency Pinning
- `next@15.1.0` (not `@latest`)
- `html-to-image@1.11.13` (not unpinned)
- Post-install version verification step

### Prompt Scope Restrictions
- "Additional instructions" scoped to visual/design changes only
- Explicit deny list: no package installs, no shell commands, no file access outside project, no network requests, no disabling security checks
- No `dangerouslySetInnerHTML`, `eval()`, `new Function()`, or `fetch()` in generated code
- Safe `<br />` rendering via JSX split pattern (not innerHTML)

### Auto-Detection Safety
- Detected values (app name, icon, screenshots) are always confirmed with the user before use
- No silent file operations based on auto-detection
- Detected file paths validated with the same rules as user-provided paths

### Installation Scope
- Project-scoped installation recommended over global installation
- Global install modifies agent behavior across all future sessions — use with caution
