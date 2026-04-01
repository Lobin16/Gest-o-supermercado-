# Testing ALFREDO ERP V13

## Application Overview
Single-file HTML/CSS/JavaScript ERP application (`index.html`). No build step, no frameworks. Open via `file:///` protocol in Chrome.

## Login
- **Master access**: Leave username blank, enter password in Senha field. Master password is stored as a Devin secret.
- Master access is required to see admin-only sections (Limites Setor, Config. IA, Armazenamento).

## Devin Secrets Needed
- `ERP_MASTER_PASSWORD`: The master login password for admin access.

## Key Testing Notes

### Data Storage
- All data is stored in `localStorage` under key `alfredoDB`.
- To start fresh: clear localStorage and reload.
- Data persists across page reloads but not across browser profile resets.

### Monkey-Patching Architecture
- New features are added via code appended at the end of `index.html`.
- Features use monkey-patching to wrap existing functions (e.g., `saveColaborador`).
- When testing monkey-patched functions, verify the patch targets the correct original function name.
- Common bug pattern: patch wrapping a non-existent function name silently fails.

### Birthday Alert Testing
- Register a collaborator with `dataNascimento` set to today's date (YYYY-MM-DD format).
- The birthday field (`rh-nascimento`) is injected by the monkey-patch into the collaborator form.
- Navigate to Dashboard to see the "Aniversariantes" card.
- The alert checks month AND day match against today's date.

### Export Escala
- Uses `prompt()` dialogs for period and format selection.
- `prompt()` dialogs may not appear visually in automated testing environments.
- Workaround: Override `window.prompt` temporarily to return desired values, then call `exportarEscalaPeriodo()` via console.
- Format options: 1 = WhatsApp (opens api.whatsapp.com), 2 = CSV (downloads file).

### Print Cardapio
- Opens a new tab/window with print-friendly layout.
- Chrome print dialog appears automatically via `window.print()`.

### AI Configuration
- Navigate to "Config. IA" in sidebar (admin-only).
- 6 providers: OpenAI, Google (Gemini), Anthropic (Claude), Mistral AI, Groq, IA Personalizada.
- Switching providers dynamically changes input field labels and placeholders.
- "IA Personalizada" shows both URL and Token fields.

### Sidebar Navigation
- Admin-only items may be clipped below the visible sidebar area on smaller viewports.
- Use `showSection('sectionName')` in console to navigate directly if nav items are not visible.
- Section names: `limites-setor`, `config-ia`, `armazenamento`.

### browser_console Tool Issues
- The `browser_console` tool may fail with "Chrome is not in the foreground" even when Chrome is visible.
- Workaround: Use DevTools console directly via F12 keyboard shortcut and type commands manually.
- Alternative: Use `xdotool search --name "ALFREDO" windowactivate --sync` before retrying.
