# Homepage Refresh Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build the approved `Profile-first + Clean Blue` homepage refresh while keeping the existing Jekyll site structure.

**Architecture:** Add a small homepage-only layout that uses the existing `default` wrapper, then move the root page content into structured semantic sections. Add one focused Sass partial for homepage presentation and import it from the existing main stylesheet.

**Tech Stack:** Jekyll, Liquid, Markdown/HTML, Sass/SCSS, Ruby one-line smoke checks.

---

## File Structure

- Create `_layouts/home.html`: homepage-only layout that wraps content in a full-width profile surface and avoids the old sidebar/article chrome.
- Modify `_pages/about.md`: switch to `layout: home` and replace Markdown-only content with semantic homepage sections.
- Create `_sass/_home.scss`: homepage-specific responsive styles.
- Modify `assets/css/main.scss`: import the new homepage partial.

## Task 1: Homepage Layout

**Files:**
- Create: `_layouts/home.html`
- Modify: `_pages/about.md`

- [ ] **Step 1: Run failing layout check**

Run:

```sh
ruby -e 'abort("missing home layout") unless File.exist?("_layouts/home.html"); about = File.read("_pages/about.md"); abort("about page does not use home layout") unless about.include?("layout: home"); puts "home layout wired"'
```

Expected: FAIL with `missing home layout`.

- [ ] **Step 2: Create `_layouts/home.html`**

Add:

```html
---
layout: default
---

<main class="home-profile" role="main">
  <article class="home-profile__container" itemscope itemtype="https://schema.org/Person">
    {{ content }}
  </article>
</main>
```

- [ ] **Step 3: Switch `_pages/about.md` front matter to the home layout**

Change the front matter from `title: "About me"` plus default layout inheritance to:

```yaml
---
permalink: /
layout: home
title: "Xiaoyin Hu"
excerpt: "Associate Professor at Shenzhen University"
author_profile: false
redirect_from:
  - /about/
  - /about.html
---
```

- [ ] **Step 4: Run layout check again**

Run:

```sh
ruby -e 'abort("missing home layout") unless File.exist?("_layouts/home.html"); about = File.read("_pages/about.md"); abort("about page does not use home layout") unless about.include?("layout: home"); puts "home layout wired"'
```

Expected: PASS with `home layout wired`.

## Task 2: Homepage Content

**Files:**
- Modify: `_pages/about.md`

- [ ] **Step 1: Run failing content check**

Run:

```sh
ruby -e 'about = File.read("_pages/about.md"); %w[home-hero home-publications home-highlights].each { |klass| abort("missing #{klass}") unless about.include?(klass) }; ["Xiaoyin Hu", "Selected Publications", "NSFC Youth Fund", "Shenzhen University"].each { |text| abort("missing #{text}") unless about.include?(text) }; puts "homepage content present"'
```

Expected: FAIL with `missing home-hero`.

- [ ] **Step 2: Replace `_pages/about.md` body with structured homepage sections**

Use the approved content:

```html
<section class="home-hero" aria-labelledby="home-title">
  <div class="home-hero__media">
    <img class="home-hero__avatar" src="{{ '/images/photo.jpg' | relative_url }}" alt="Xiaoyin Hu">
    <div class="home-hero__actions" aria-label="Profile links">
      <a class="home-button home-button--primary" href="{{ '/publications/' | relative_url }}">Publications</a>
      <a class="home-button" href="https://scholar.google.com/citations?user=vw7ahnAAAAAJ">Google Scholar</a>
      <a class="home-button" href="mailto:hxy@amss.ac.cn">Email</a>
    </div>
  </div>

  <div class="home-hero__content">
    <p class="home-eyebrow">School of Mathematical Sciences, Shenzhen University</p>
    <h1 id="home-title">Xiaoyin Hu</h1>
    <p class="home-role">Associate Professor, School of Mathematical Sciences, Shenzhen University</p>
    <p class="home-summary">I work on nonsmooth optimization, Riemannian optimization, and optimization problems in machine learning.</p>
    <div class="home-tags" aria-label="Research interests">
      <span>Nonsmooth optimization</span>
      <span>Riemannian optimization</span>
      <span>Machine learning</span>
    </div>
  </div>
</section>

<section class="home-section home-publications" aria-labelledby="selected-publications">
  <div class="home-section__header">
    <h2 id="selected-publications">Selected Publications</h2>
    <a href="{{ '/publications/' | relative_url }}">View all</a>
  </div>
  <div class="home-publication-list">
    <article class="home-publication">
      <h3><a href="https://arxiv.org/pdf/2305.03938">Adam-family Methods for Nonsmooth Optimization with Convergence Guarantees</a></h3>
      <p>Nachuan Xiao, <strong>Xiaoyin Hu</strong>, Xin Liu, Kim-Chuan Toh. Journal of Machine Learning Research, 2024.</p>
    </article>
    <article class="home-publication">
      <h3><a href="https://link.springer.com/article/10.1007/s12532-025-00277-z">CDOpt: a Python package for a class of Riemannian optimization</a></h3>
      <p>Nachuan Xiao, <strong>Xiaoyin Hu</strong>, Xin Liu, Kim-Chuan Toh. Mathematical Programming Computation, 2025.</p>
    </article>
    <article class="home-publication">
      <h3><a href="https://arxiv.org/pdf/2408.17213">A minimization approach for minimax optimization with coupled constraints</a></h3>
      <p>Xiaoyin Hu, Kim-Chuan Toh, Shiwei Wang, Nachuan Xiao. SIAM Journal on Optimization, 2026.</p>
    </article>
  </div>
</section>

<section class="home-section home-highlights" aria-label="Academic highlights">
  <article class="home-highlight home-highlight--dark">
    <p class="home-highlight__label">Current Grant</p>
    <h2>深度学习中的流形优化问题：算法设计与求解软件包的开发</h2>
    <p>NSFC Youth Fund, 2024.01-2026.12</p>
  </article>
  <article class="home-highlight">
    <p class="home-highlight__label">Current Position</p>
    <h2>Shenzhen University</h2>
    <p>School of Mathematical Sciences, 2025.10-present</p>
  </article>
</section>
```

- [ ] **Step 3: Run content check again**

Run:

```sh
ruby -e 'about = File.read("_pages/about.md"); %w[home-hero home-publications home-highlights].each { |klass| abort("missing #{klass}") unless about.include?(klass) }; ["Xiaoyin Hu", "Selected Publications", "NSFC Youth Fund", "Shenzhen University"].each { |text| abort("missing #{text}") unless about.include?(text) }; puts "homepage content present"'
```

Expected: PASS with `homepage content present`.

## Task 3: Homepage Styling

**Files:**
- Create: `_sass/_home.scss`
- Modify: `assets/css/main.scss`

- [ ] **Step 1: Run failing style check**

Run:

```sh
ruby -e 'abort("missing home styles") unless File.exist?("_sass/_home.scss"); main = File.read("assets/css/main.scss"); abort("home styles not imported") unless main.include?("@import \"home\";"); styles = File.read("_sass/_home.scss"); %w[home-profile home-hero home-publications home-highlights].each { |klass| abort("missing #{klass} styles") unless styles.include?(".#{klass}") }; puts "home styles wired"'
```

Expected: FAIL with `missing home styles`.

- [ ] **Step 2: Add `_sass/_home.scss`**

Add:

```scss
/* ==========================================================================
   HOMEPAGE PROFILE
   ========================================================================== */

.home-profile {
  min-height: calc(100vh - 75px);
  padding: 2.5rem 1rem 4rem;
  background: #f8fbff;
}

.home-profile__container {
  max-width: 1120px;
  margin: 0 auto;
}

.home-hero {
  display: grid;
  grid-template-columns: 170px minmax(0, 1fr);
  gap: 2rem;
  align-items: start;
  padding: 2.25rem 0 2rem;
}

.home-hero__media {
  display: grid;
  justify-items: center;
  gap: 1rem;
}

.home-hero__avatar {
  width: 132px;
  height: 132px;
  border: 6px solid #fff;
  border-radius: 50%;
  object-fit: cover;
  box-shadow: 0 18px 44px rgba(37, 99, 235, 0.18);
}

.home-hero__actions {
  display: grid;
  width: 100%;
  max-width: 160px;
  gap: 0.55rem;
}

.home-button {
  display: inline-flex;
  min-height: 2.25rem;
  align-items: center;
  justify-content: center;
  padding: 0.45rem 0.75rem;
  border: 1px solid #cbd5e1;
  border-radius: 6px;
  background: #fff;
  color: #334155;
  font-family: $sans-serif;
  font-size: $type-size-6;
  font-weight: 700;
  line-height: 1.2;
  text-align: center;
  text-decoration: none;

  &:hover {
    color: #1d4ed8;
    text-decoration: none;
    border-color: #93c5fd;
  }
}

.home-button--primary {
  border-color: #2563eb;
  background: #2563eb;
  color: #fff;

  &:hover {
    color: #fff;
    background: #1d4ed8;
  }
}

.home-eyebrow {
  margin: 0 0 0.6rem;
  color: #2563eb;
  font-family: $sans-serif;
  font-size: $type-size-6;
  font-weight: 800;
  letter-spacing: 0;
}

.home-hero__content h1 {
  margin: 0;
  color: #0f172a;
  font-family: $sans-serif;
  font-size: 2.7rem;
  line-height: 1.05;
}

.home-role {
  margin: 0.7rem 0 0;
  color: #475569;
  font-family: $sans-serif;
  font-size: $type-size-5;
}

.home-summary {
  max-width: 720px;
  margin: 1rem 0 0;
  color: #334155;
  font-family: $sans-serif;
  font-size: $type-size-5;
  line-height: 1.65;
}

.home-tags {
  display: flex;
  flex-wrap: wrap;
  gap: 0.55rem;
  margin-top: 1.2rem;

  span {
    display: inline-flex;
    align-items: center;
    min-height: 2rem;
    padding: 0.3rem 0.75rem;
    border: 1px solid #bae6fd;
    border-radius: 999px;
    background: #e0f2fe;
    color: #075985;
    font-family: $sans-serif;
    font-size: $type-size-6;
    font-weight: 700;

    &:nth-child(2) {
      border-color: #c7d2fe;
      background: #eef2ff;
      color: #3730a3;
    }

    &:nth-child(3) {
      border-color: #bbf7d0;
      background: #ecfdf5;
      color: #047857;
    }
  }
}

.home-section {
  margin-top: 1.5rem;
}

.home-section__header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 1rem;
  margin-bottom: 1rem;

  h2 {
    margin: 0;
    color: #0f172a;
    font-family: $sans-serif;
    font-size: $type-size-4;
  }

  a {
    color: #2563eb;
    font-family: $sans-serif;
    font-size: $type-size-6;
    font-weight: 800;
    text-decoration: none;

    &:hover {
      text-decoration: underline;
    }
  }
}

.home-publications {
  padding: 1.25rem;
  border: 1px solid #dbe6f3;
  border-radius: 8px;
  background: #fff;
  box-shadow: 0 14px 36px rgba(15, 23, 42, 0.05);
}

.home-publication-list {
  display: grid;
  gap: 1rem;
}

.home-publication {
  padding-left: 0.9rem;
  border-left: 3px solid #60a5fa;

  &:first-child {
    border-left-color: #2563eb;
  }

  h3 {
    margin: 0;
    font-family: $sans-serif;
    font-size: $type-size-5;
    line-height: 1.35;

    a {
      color: #0f172a;
      text-decoration: none;

      &:hover {
        color: #2563eb;
        text-decoration: underline;
      }
    }
  }

  p {
    margin: 0.3rem 0 0;
    color: #64748b;
    font-family: $sans-serif;
    font-size: $type-size-6;
    line-height: 1.5;
  }
}

.home-highlights {
  display: grid;
  grid-template-columns: minmax(0, 1.1fr) minmax(0, 0.9fr);
  gap: 1rem;
}

.home-highlight {
  padding: 1.25rem;
  border: 1px solid #dbe6f3;
  border-radius: 8px;
  background: #fff;

  h2 {
    margin: 0.35rem 0 0;
    color: #0f172a;
    font-family: $sans-serif;
    font-size: $type-size-5;
    line-height: 1.45;
  }

  p {
    margin: 0.55rem 0 0;
    color: #64748b;
    font-family: $sans-serif;
    font-size: $type-size-6;
    line-height: 1.45;
  }
}

.home-highlight__label {
  margin: 0 !important;
  color: #2563eb !important;
  font-family: $sans-serif;
  font-size: $type-size-7 !important;
  font-weight: 800;
  letter-spacing: 0;
  text-transform: uppercase;
}

.home-highlight--dark {
  border-color: #172033;
  background: #172033;
  color: #fff;

  h2 {
    color: #fff;
  }

  p {
    color: #cbd5e1;
  }

  .home-highlight__label {
    color: #93c5fd !important;
  }
}

@include breakpoint(max-width $medium) {
  .home-profile {
    padding: 1.5rem 1rem 3rem;
  }

  .home-hero,
  .home-highlights {
    grid-template-columns: 1fr;
  }

  .home-hero {
    gap: 1.5rem;
    padding-top: 1.5rem;
  }

  .home-hero__media {
    justify-items: start;
  }

  .home-hero__actions {
    max-width: none;
    grid-template-columns: repeat(3, minmax(0, 1fr));
  }

  .home-hero__content h1 {
    font-size: 2.2rem;
  }
}

@include breakpoint(max-width $small) {
  .home-hero__actions {
    grid-template-columns: 1fr;
  }

  .home-section__header {
    align-items: flex-start;
    flex-direction: column;
  }
}
```

- [ ] **Step 3: Import the Sass partial**

Add this line in `assets/css/main.scss` after `@import "sidebar";`:

```scss
@import "home";
```

- [ ] **Step 4: Run style check again**

Run:

```sh
ruby -e 'abort("missing home styles") unless File.exist?("_sass/_home.scss"); main = File.read("assets/css/main.scss"); abort("home styles not imported") unless main.include?("@import \"home\";"); styles = File.read("_sass/_home.scss"); %w[home-profile home-hero home-publications home-highlights].each { |klass| abort("missing #{klass} styles") unless styles.include?(".#{klass}") }; puts "home styles wired"'
```

Expected: PASS with `home styles wired`.

## Task 4: Verification

**Files:**
- Verify: `_layouts/home.html`
- Verify: `_pages/about.md`
- Verify: `_sass/_home.scss`
- Verify: `assets/css/main.scss`

- [ ] **Step 1: Run YAML check**

Run:

```sh
ruby -e 'require "yaml"; YAML.load_file("_config.yml"); YAML.load_file("_data/navigation.yml"); puts "yaml ok"'
```

Expected: PASS with `yaml ok`.

- [ ] **Step 2: Run include/layout reference checks**

Run:

```sh
ruby -e 'missing=[]; Dir.glob("**/*.{html,md}", File::FNM_EXTGLOB).each do |f|; next if f.start_with?("_site/") || f.start_with?(".superpowers/"); text=File.read(f); text.scan(/\{%\s*include\s+([^\s%]+).*?%\}/).flatten.each do |inc|; inc=inc.delete("\"'\''").sub(%r{^/}, ""); path=File.join("_includes", inc); missing << [f, inc] unless File.exist?(path); end; end; abort(missing.map { |f,i| "#{f}: missing include #{i}" }.join("\n")) unless missing.empty?; puts "includes ok"'
```

Expected: PASS with `includes ok`.

Run:

```sh
ruby -e 'require "yaml"; missing=[]; Dir.glob("{_pages,_layouts}/**/*.{md,html}", File::FNM_EXTGLOB).each do |f|; text=File.read(f); if text =~ /\A---\s*\n(.*?)\n---/m; y=YAML.safe_load($1, permitted_classes: [], aliases: true) rescue {}; layout=y && y["layout"]; missing << [f, layout] if layout && !File.exist?("_layouts/#{layout}.html"); end; end; abort(missing.map { |f,l| "#{f}: missing layout #{l}" }.join("\n")) unless missing.empty?; puts "layouts ok"'
```

Expected: PASS with `layouts ok`.

- [ ] **Step 3: Run diff whitespace check**

Run:

```sh
git diff --check
```

Expected: PASS with no output.

- [ ] **Step 4: Attempt local Jekyll build**

Run:

```sh
BUNDLE_APP_CONFIG=/tmp/codex-homepage-bundle-config BUNDLE_PATH=/tmp/codex-homepage-bundle bundle install && BUNDLE_APP_CONFIG=/tmp/codex-homepage-bundle-config BUNDLE_PATH=/tmp/codex-homepage-bundle bundle exec jekyll build
```

Expected: PASS if Ruby 3-compatible dependencies can install. If it fails because local Ruby is `2.6.10` and current `github-pages` requires Ruby 3.x, record the blocker and rely on the structural checks above.
