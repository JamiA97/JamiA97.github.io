
---
title: "Offline Instructions for Managing Sections"
---

## 🏠 Add a New Section to the Homepage

1. Open `layouts/partials/header.html`
2. Add a new link inside the `<nav>` block:
   ```html
   <a href='/newsection'>/newsection</a>
   ```
3. Create a folder in `content/`:
   ```bash
   mkdir content/newsection
   echo -e "---\ntitle: \"New Section\"\n---" > content/newsection/_index.md
   ```

## 📂 Add a New Section Under `/notebook`

1. Create a new subfolder:
   ```bash
   mkdir content/notebook/logic
   ```

2. Add `_index.md` for Hugo to recognize the section:
   ```bash
   echo -e "---\ntitle: \"Logic\"\n---" > content/notebook/logic/_index.md
   ```

3. To add entries, place Markdown files in `content/notebook/logic/`

## 🏷️ Add Tags to Entries

In the front matter of any `.md` file, include:
```yaml
tags: ["philosophy", "math"]
```

The site will auto-generate pages like `/tags/philosophy/` showing all posts with that tag.

## 🚀 Tips

- Use `hugo server` to preview changes locally.
- Commit changes to Git using:
  ```bash
  git add .
  git commit -m "Add new section"
  git push
  ```
