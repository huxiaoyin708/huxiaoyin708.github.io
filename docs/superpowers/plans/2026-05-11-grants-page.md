# Grants Page Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Add a dedicated `/grants/` page with the full grants list while keeping the homepage focused on the current grant.

**Architecture:** Use the existing Jekyll `_pages` collection and `archive` layout for the new Grants page. Keep the homepage `home-highlight` card as a short current-grant summary and link it to the full page. Add Grants to the existing main navigation.

**Tech Stack:** Jekyll, Markdown/HTML, Liquid `relative_url`, Sass already present for homepage cards.

---

## File Structure

- Create `_pages/grants.md`: full grants list page using the existing archive layout.
- Modify `_pages/about.md`: change the Funding card back to one current grant and add a link to `/grants/`.
- Modify `_data/navigation.yml`: add Grants between Publications and Experience.
- Modify `_sass/_home.scss`: style the homepage Grants link and remove now-unused full-list homepage grant styles.

## Task 1: Regression Check

**Files:**
- Check: `_pages/about.md`
- Check: `_pages/grants.md`
- Check: `_data/navigation.yml`

- [ ] **Step 1: Run failing grants page check**

Run:

```sh
ruby -e 'abort("missing grants page") unless File.exist?("_pages/grants.md"); grants = File.read("_pages/grants.md"); ["深度学习中的流形优化问题：算法设计与求解软件包的开发", "机器学习中的流形优化问题的算法设计及其理论与应用", "正交约束优化问题的算法设计及其在人工智能中的应用"].each { |text| abort("missing grant: #{text}") unless grants.include?(text) }; about = File.read("_pages/about.md"); abort("homepage should link to grants") unless about.include?("'/grants/'") || about.include?("\"/grants/\""); nav = File.read("_data/navigation.yml"); abort("navigation missing Grants") unless nav.include?("title: \"Grants\"") && nav.include?("url: /grants/"); puts "grants page wired"'
```

Expected: FAIL with `missing grants page`.

## Task 2: Add Grants Page

**Files:**
- Create: `_pages/grants.md`

- [ ] **Step 1: Create `_pages/grants.md`**

Add:

```markdown
---
layout: archive
title: "Grants"
permalink: /grants/
author_profile: true
---

### Grants

------

* 2024.01-2026.12<br>
  深度学习中的流形优化问题：算法设计与求解软件包的开发，国家自然科学基金委员会，青年基金，主持.<br>
* 2023.01-2025.12<br>
  机器学习中的流形优化问题的算法设计及其理论与应用，浙江省自然科学基金委员会，探索青年，主持.（结题）<br>
* 2022.10-2024.10<br>
  正交约束优化问题的算法设计及其在人工智能中的应用，浙江省教育厅，一般项目，主持.（结题）<br>
```

## Task 3: Update Homepage

**Files:**
- Modify: `_pages/about.md`

- [ ] **Step 1: Replace the full homepage grants list with current grant summary**

The dark Funding card should contain:

```html
<p class="home-highlight__label">Current Grant</p>
<h2>深度学习中的流形优化问题：算法设计与求解软件包的开发</h2>
<p>国家自然科学基金委员会，青年基金，主持. 2024.01-2026.12</p>
<a class="home-highlight__link" href="{{ '/grants/' | relative_url }}">View all grants</a>
```

## Task 4: Update Navigation

**Files:**
- Modify: `_data/navigation.yml`
- Modify: `_sass/_home.scss`

- [ ] **Step 1: Add Grants navigation item**

The main navigation should include:

```yaml
  - title: "Grants"
    url: /grants/
```

between Publications and Experience.

- [ ] **Step 2: Style the homepage grants link**

Add a `.home-highlight__link` rule in `_sass/_home.scss` so the link reads as a compact action inside the dark card:

```scss
.home-highlight__link {
  display: inline-flex;
  min-height: 2.25rem;
  align-items: center;
  justify-content: center;
  margin-top: 0.85rem;
  padding: 0.45rem 0.75rem;
  border: 1px solid #93c5fd;
  border-radius: 6px;
  color: #dbeafe;
  font-family: $sans-serif;
  font-size: $type-size-6;
  font-weight: 800;
  line-height: 1.2;
  text-decoration: none;

  &:hover {
    border-color: #bfdbfe;
    color: #fff;
    text-decoration: none;
  }
}
```

- [ ] **Step 3: Remove unused full-list homepage grant styles**

Delete the `.home-grant-list`, `.home-grant`, `.home-grant__date`, `.home-grant__title`, and `.home-grant__meta` rules from `_sass/_home.scss` because the complete grants list now lives on `_pages/grants.md`.

## Task 5: Verification

**Files:**
- Check: `_pages/grants.md`
- Check: `_pages/about.md`
- Check: `_data/navigation.yml`

- [ ] **Step 1: Run the grants page check**

Run the command from Task 1 again.

Expected: PASS with `grants page wired`.

- [ ] **Step 2: Run YAML and whitespace checks**

Run:

```sh
ruby -e 'require "yaml"; YAML.load_file("_config.yml"); YAML.load_file("_data/navigation.yml"); puts "yaml ok"'
git diff --check
```

Expected: PASS with `yaml ok` and no whitespace errors.
