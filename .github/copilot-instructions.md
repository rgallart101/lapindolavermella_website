# Copilot Instructions for La Píndola Vermella Website

## Project Overview

This is a **Pelican static site generator** for "La Píndola Vermella," a Catalan-language podcast about digital safety and technology. The site uses the **Flex theme** and is managed via Pelican tasks.

**Key architecture:**
- **Content**: Markdown files in `content/` with metadata (Title, Date)
- **Generation**: Pelican converts Markdown → HTML to `output/`
- **Theme**: External Flex theme (must be installed separately)
- **Deployment**: SSH/rsync-based for remote hosting

## Content Structure

Content files follow naming convention: `lpv_s{season}e{episode}_{slug}.md` (e.g., `lpv_s01e01_presentacio.md`)

Each Markdown file must include front matter:
```markdown
Title: Episode Title
Date: YYYY-MM-DD HH:MM
```

Current structure spans 3 seasons (10 episodes each) + bonus content. Episodes cover digital safety topics progressively.

## Development Workflow

### Initial Setup
```bash
python -m venv .venv
source ./.venv/bin/activate
pip install -r requirements.txt
```

Install Flex theme (one-time):
```bash
git clone https://github.com/getpelican/pelican-themes
pelican-themes --install <path>/Flex
```

### Key Commands (via `tasks.py` using Invoke)
- `invoke build`: Generate site to `output/`
- `invoke serve`: Serve locally at localhost:8000
- `invoke regenerate`: Auto-rebuild on content changes
- `invoke preview`: Build with production settings (`publishconf.py`)
- `invoke livereload`: Build + auto-reload browser on changes
- `invoke publish`: Generate + deploy via rsync+SSH

### Makefile Alternative
- `make html`: Generate site
- `make devserver [PORT=8000]`: Serve + regenerate together
- `make serve-global [SERVER=0.0.0.0]`: Serve on 0.0.0.0:80

## Configuration Files

**`pelicanconf.py`**: Local development settings
- `SITEURL = ""` (empty for local testing)
- `DEFAULT_LANG = 'ca'` (Catalan)
- Theme: `"Flex"`
- Feeds disabled during development

**`publishconf.py`**: Production settings (imported via `pelicanconf.py`)
- `SITEURL = "https://www.lapindolavermella.cat"`
- Enables Atom feeds
- `DELETE_OUTPUT_DIRECTORY = True` (clean rebuild)

**`Makefile` & `tasks.py`**: Build orchestration
- Environment variables for SSH deployment: `ENV_SSH_HOST`, `ENV_SSH_PORT`, `ENV_SSH_USER`, `ENV_SSH_TARGET_DIR`
- Create `.env` file for remote upload (see `upload_remote` script)

## Project Conventions

1. **Content language**: All site content in Catalan (`ca`)
2. **Episode naming**: Season/episode codes embedded in filename for logical organization
3. **Deployment**: Local builds to `output/`, then rsync via SSH to remote
4. **Theme customization**: Flex theme files in `output/theme/` after generation (don't edit directly—modify theme source)

## When Adding/Modifying Content

- Add `.md` files to `content/` with proper date format
- Embed media directly in Markdown (e.g., YouTube iframes)
- Run `invoke livereload` during editing for instant feedback
- Build with `invoke preview` to test production settings before deploying
- Use `invoke publish` for final deployment

## Output Structure

Generated `output/` contains:
- Static HTML pages (`index.html`, `archives.html`, etc.)
- Theme assets in `theme/` (CSS, fonts, JavaScript)
- Feeds in `feeds/` (Atom XML)
- Images in `images/` (copied from `content/images/`)

Do not commit `output/` to version control—regenerate on deployment.
