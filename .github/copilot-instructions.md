<!-- .github/copilot-instructions.md: Guidance for AI coding agents working in this repo -->
# Copilot instructions for this repository

Purpose: give an AI coding agent the minimal, concrete knowledge needed to be productive editing, extending, and testing this static website project.

- Project type: plain static site (HTML/CSS/JS) with a small PHP form handler.
- Primary entry points: `index.html`, `portfolio-details.html`, `service-details.html`.
- Static assets: `assets/css/main.css`, `assets/js/main.js`, `assets/img/` and vendor libraries under `assets/vendor/`.

Practical goals for edits
- Change site content: update the root HTML files in the repo root (e.g. `index.html`) and update styles in `assets/css/main.css`.
- Add/modify JS behavior: edit `assets/js/main.js`. Third‑party libs are under `assets/vendor/` and are referenced directly from the HTML.
- Update contact form behavior: `forms/contact.php` is the server‑side handler. Testing changes to it requires running a PHP server (see Testing below).

Project conventions and important patterns
- No build tool detected: the repo serves files as‑is. Do not add build steps unless the user requests it.
- Vendor libraries are preserved in `assets/vendor/` (Bootstrap, glightbox, swiper, typed.js, isotope, etc.). Prefer adding small overrides in `assets/css/main.css` rather than editing vendor files.
- SCSS folder exists (`assets/scss/`), but there is no build script in the repo. If asked to switch to compiled SCSS, clarify whether to add a toolchain (node/sass).
- Images follow organized folders: `assets/img/portfolio/`, `assets/img/testimonials/` — keep those paths when updating markup.

Data flows & integration points
- Frontend -> backend: contact form posts to `forms/contact.php`. That file prepares and sends email (or returns a response). To test email sending locally, you must configure a PHP mail transport or mock that behavior.
- HTML references vendor JS/CSS by relative paths under `assets/vendor/`. When adding a new library, place it in `assets/vendor/<lib>/` and add tags in the affected HTML file.

Testing and local workflows (commands that work for this repo)
- Serve static files quickly (no PHP required):
  - `python3 -m http.server 8000` (open `http://localhost:8000`)
- Serve with PHP (to test `forms/contact.php`):
  - `php -S localhost:8000 -t .` then open `http://localhost:8000` and submit the contact form.
- Browser debugging: use browser console + Network tab to confirm script and asset loads (check paths such as `assets/vendor/...`).

Editing guidelines for PRs
- Keep changes minimal and scoped: editing `index.html` should not modify unrelated pages unless needed.
- Prefer CSS overrides in `assets/css/main.css` instead of modifying vendor CSS files in `assets/vendor/`.
- If adding JS, use `assets/js/main.js` or add a new file under `assets/js/` and reference it from the appropriate HTML page.

Examples (concrete edits)
- To change the hero heading on the home page: edit `index.html` (look for the `.hero` section) and adjust styles in `assets/css/main.css`.
- To add a new carousel using Swiper: add library files under `assets/vendor/swiper/` and initialize in `assets/js/main.js` (follow existing patterns in that file).
- To change contact processing behavior: edit `forms/contact.php`. Test locally with `php -S` and inspect the Network tab for the form POST to `/forms/contact.php`.

What the agent must not assume
- Do not assume a node/npm toolchain is present — ask before introducing build tooling.
- Do not modify or delete `assets/vendor/` files without confirming with the user.

If something is unclear, ask these short clarifying questions
1. Should I add a build tool (npm/sass/webpack) if converting SCSS to CSS? (yes/no)
2. Is there an expected email transport configuration for `forms/contact.php` for local testing?
3. Are we deploying to a specific host (Vercel, Netlify) that requires special routing or headers?

End of file — please review and tell me any missing repo details to include.
