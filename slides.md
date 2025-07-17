---
marp: true
theme: default
paginate: true
---

# Welcome to create-marp-slides!

🚀 **Quick Setup Tool for Marp Presentations**

Your presentation project is ready to go!

---

## What is create-marp-slides?

- **🏗️ Scaffolding Tool**: Create Marp presentation projects instantly
- **📝 Markdown-based**: Write slides in Markdown with Marp syntax
- **🔄 Auto-deploy**: GitHub Actions for automatic GitHub Pages deployment
- **🎨 Zero Configuration**: Works out of the box with sensible defaults

---

## How to Use

```bash
# Create a new presentation project
npx create-marp-slides my-presentation

# Navigate to your project
cd my-presentation

# Install dependencies
npm install

# Start development server
npm run dev
```

---

## Generated Project Structure

```
my-presentation/
├── slides.md              # Your presentation content (this file!)
├── package.json           # Project configuration
├── .gitignore             # Git ignore rules
└── .github/
    └── workflows/
        └── deploy.yml      # GitHub Actions for auto-deployment
```

---

## Development Commands

- **`npm run dev`** - Start development server with live reload
- **`npm run build`** - Build slides to static HTML
- **`npm run preview`** - Preview built slides locally

All powered by [@marp-team/marp-cli](https://github.com/marp-team/marp-cli)

---

## GitHub Pages Auto-Deployment

1. Push your project to GitHub
2. Go to **Settings > Pages**
3. Set Source to **"GitHub Actions"**
4. Edit `slides.md` and push changes
5. Your slides will be automatically deployed! 🎉

---

## Math Support

Inline math: $E = mc^2$

Block math:
$$
\sum_{i=1}^{n} i = \frac{n(n+1)}{2}
$$

Perfect for technical presentations!

---

## Code Highlighting

```javascript
// Create slides programmatically
function createSlides() {
  const markdown = '# Hello create-marp-slides!';
  return marp.render(markdown);
}

// Start your presentation journey
createSlides();
```

---

## Images and Media

![Marp](https://marp.app/assets/marp.svg)

Supports images, videos, and rich media content.

---

## Get Started Now!

1. **Edit this file** (`slides.md`) to create your own slides
2. **Use `---` to separate slides**
3. **Push to GitHub** for automatic deployment
4. **Share your presentation** with the world!

Happy presenting! 🎉