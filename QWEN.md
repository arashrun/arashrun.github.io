# Shivers Blog - Hexo Site

## Project Overview
This is a personal blog called "Shivers" built using Hexo, a popular static site generator written in Node.js. The site belongs to user "arashrun" and is hosted at https://arashrun.github.io/. It serves as a digital garden for documenting and sharing technical knowledge, programming concepts, and learning experiences.

Key characteristics:
- Built with Hexo v5.4.2
- Uses a custom theme named "self-hexo-theme"
- Hosted on GitHub Pages as a user site
- Rich technical content covering C++, Python, Linux, Qt, web development, and various programming topics
- Contains 70+ detailed posts on technical subjects
- Integrated with Obsidian workflow (as mentioned in index.md)

## Technologies Used
- **Hexo**: Static site generator
- **Node.js**: Runtime environment
- **Markdown**: Content authoring format
- **Git/GitHub**: Version control and deployment
- **Custom Theme**: self-hexo-theme (appears to be customized)

## Site Structure
```
├── _config.yml          # Main Hexo configuration
├── package.json         # Node.js dependencies and scripts
├── scaffolds/           # Post/page templates
├── source/              # Source content
│   ├── about.md         # About page
│   ├── index.md         # Homepage
│   ├── search.md        # Search page
│   ├── _posts/          # Blog posts (70+ technical articles)
│   ├── _模板/           # Templates
│   └── other dirs       # Categories, tags, images, etc.
└── themes/              # Theme directory
    └── self-hexo-theme  # Custom theme
```

## Building and Running

### Development Commands
```bash
# Install dependencies
npm install

# Start local development server
npm run server
# Alternative: npx hexo server

# Generate static files
npm run build
# Alternative: npx hexo generate

# Clean generated files
npm run clean
# Alternative: npx hexo clean

# Deploy to GitHub Pages
npm run deploy
# Alternative: npx hexo deploy
```

### Key Configuration
- Site URL: https://arashrun.github.io/
- Permalink format: `:year/:month/:day/:title/`
- Default posts per page: 10
- Theme: `self-hexo-theme`
- Content is written in Markdown
- Deployment target: GitHub Pages (`arashrun/arashrun.github.io.git`, master branch)

## Content Organization
The blog contains extensive technical content organized by:
- **Categories**: Organized by technology topics (linux, C++, python, etc.)
- **Tags**: Keywords for specific concepts
- **Posts**: Located in `source/_posts/` directory with front-matter metadata

Topics covered include:
- System Programming (C++, Linux internals)
- Web Development (JavaScript, React, TypeScript)
- Mobile Development (Android, Qt)
- Databases, Networking, and Security
- Build Systems (CMake, Makefile, Meson)
- Development Tools (Vim, Git, Docker)

## Development Conventions
- New posts follow the filename convention specified in `_config.yml`
- Markdown formatting with special highlighting conventions:
  - `<u>Green content</u>`: Represents concepts and terminology
  - `==Yellow content==`: Highlights important keywords
  - `[Content]`: Unfamiliar terms marked for investigation

## Special Features
- Integration with Obsidian for note-taking workflow
- Custom Hexo theme for personalized appearance
- Support for code highlighting and various plugins
- Automated deployment to GitHub Pages
- Plugin support for enhanced Markdown capabilities (markdown-it-checkbox, mermaid diagrams)

## Deployment
The site uses GitHub Pages for hosting with automatic deployment through Hexo's git deployer. The generated static files are pushed to the master branch of the repository.

## Dependencies
See `package.json` for the complete list of dependencies including:
- hexo-core
- Various generators (archive, category, tag, index)
- Renderers (EJS, markdown-it-plus, stylus)
- Server utilities