# Haven Tile & Stone — Full Website Build Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build and launch a full WordPress website for Haven Tile & Stone with a Light & Coastal Luxury aesthetic, 4 service pages, 8 location landing pages, a portfolio gallery, lead capture forms, and a technical SEO foundation for an ongoing local SEO campaign.

**Architecture:** WordPress CMS with a lightweight custom child theme (based on GeneratePress or Kadence) providing the design system from the approved mockup. ACF (Advanced Custom Fields) powers flexible content areas on service and location pages. RankMath handles all on-page SEO and schema. WPForms handles lead capture with email notifications to Donovan.

**Tech Stack:** WordPress 6.x · GeneratePress/Kadence theme · ACF Pro · RankMath Pro · WPForms · Cloudflare (CDN/DNS) · WP Rocket (caching) · UpdraftPlus (backups)

---

## Phase 1 — Local Development Environment

### Task 1: Install Local WordPress Environment

**Files:**
- Local by Flywheel app (download from localwp.com)
- Site root: `~/Local Sites/haven-tile-stone/`

- [ ] **Step 1: Download and install Local by Flywheel**

Go to https://localwp.com and download for Windows. Install it. This gives you a full WordPress environment (PHP, MySQL, Nginx) on your machine with one click.

- [ ] **Step 2: Create new site in Local**

Open Local → Create new site → Site name: `haven-tile-stone` → Choose "Preferred" environment → WordPress username: `admin`, password: `admin`, email: your email → Create site.

- [ ] **Step 3: Open the site admin**

In Local, click "WP Admin" → logs in automatically. You'll see the WordPress dashboard at `http://haven-tile-stone.local/wp-admin`.

- [ ] **Step 4: Open site in browser**

Click "Open Site" in Local → confirms `http://haven-tile-stone.local` loads the default WordPress theme.

- [ ] **Step 5: Commit project scaffold**

```bash
cd ~/Local\ Sites/haven-tile-stone/app/public
git init
echo "wp-content/uploads/" >> .gitignore
echo "wp-config.php" >> .gitignore
git add .gitignore
git commit -m "chore: init haven tile and stone wordpress project"
```

---

### Task 2: Install and Activate Core Plugins

**Files:**
- `wp-content/plugins/` (managed via WP Admin)

- [ ] **Step 1: Install GeneratePress theme**

WP Admin → Appearance → Themes → Add New → search "GeneratePress" → Install → Activate.

- [ ] **Step 2: Install GeneratePress Premium (if licensed)**

Upload the GeneratePress Premium plugin zip → Plugins → Add New → Upload Plugin → activate it. If no license, Kadence (free) is a solid alternative — same process.

- [ ] **Step 3: Install RankMath SEO**

Plugins → Add New → search "Rank Math SEO" → Install → Activate → run setup wizard → connect to Google Search Console when prompted (can skip for now).

- [ ] **Step 4: Install WPForms Lite**

Plugins → Add New → search "WPForms" → Install → Activate.

- [ ] **Step 5: Install Advanced Custom Fields**

Plugins → Add New → search "Advanced Custom Fields" → Install → Activate.

- [ ] **Step 6: Install WP Rocket (or LiteSpeed Cache as free alternative)**

Upload WP Rocket zip (paid, recommended) or install "LiteSpeed Cache" free via plugin search → Activate.

- [ ] **Step 7: Install UpdraftPlus**

Plugins → Add New → search "UpdraftPlus" → Install → Activate.

- [ ] **Step 8: Verify plugin list**

Confirm all plugins show Active in WP Admin → Plugins. Expected active list:
- GeneratePress (theme)
- Rank Math SEO
- WPForms Lite
- Advanced Custom Fields
- WP Rocket / LiteSpeed Cache
- UpdraftPlus

---

## Phase 2 — Design System & Theme Setup

### Task 3: Configure Global Design Tokens in GeneratePress

**Files:**
- WP Admin → Appearance → Customize → GeneratePress settings

- [ ] **Step 1: Set global colors**

Appearance → Customize → Colors → set:
- Background: `#FDFAF5`
- Text: `#1C1C1C`
- Link: `#2A3F3C`
- Link Hover: `#C8A96A`
- Accent: `#C8A96A`

- [ ] **Step 2: Set global typography**

Appearance → Customize → Typography:
- Body font: DM Sans, weight 300, size 16px, line-height 1.7
- H1–H3: Playfair Display, weight 600
- H4–H6: DM Sans, weight 500, letter-spacing 0.08em

Add Google Fonts: paste these in Appearance → Customize → Additional CSS:
```css
@import url('https://fonts.googleapis.com/css2?family=Playfair+Display:ital,wght@0,400;0,600;0,700;1,400;1,500&family=DM+Sans:wght@300;400;500&display=swap');
```

- [ ] **Step 3: Set layout widths**

Appearance → Customize → Layout → Container: 1280px max width, no sidebar (full width default).

- [ ] **Step 4: Configure navigation**

Appearance → Customize → Navigation → menu position: right, transparent header overlay on homepage hero.

- [ ] **Step 5: Create custom CSS file**

Create `wp-content/themes/generatepress-child/style.css` with:

```css
/*
Theme Name: Haven Tile Child
Template: generatepress
*/

:root {
  --sand:    #F5F0E8;
  --cream:   #FDFAF5;
  --teal:    #2A3F3C;
  --teal-lt: #3D5954;
  --gold:    #C8A96A;
  --charcoal:#1C1C1C;
  --gray:    #6B6B6B;
  --border:  #E2DDD4;
}

/* Global resets */
body { font-family: 'DM Sans', sans-serif; background: var(--cream); }
h1, h2, h3, h4 { font-family: 'Playfair Display', serif; }

/* Buttons */
.btn-primary {
  background: var(--teal); color: #fff;
  padding: 14px 34px; font-size: 0.82rem;
  letter-spacing: 0.1em; text-transform: uppercase;
  display: inline-block; text-decoration: none;
  transition: background 0.2s;
}
.btn-primary:hover { background: var(--teal-lt); color: #fff; }

.btn-outline {
  border: 1.5px solid var(--teal); color: var(--teal);
  padding: 14px 34px; font-size: 0.82rem;
  letter-spacing: 0.1em; text-transform: uppercase;
  display: inline-block; text-decoration: none;
  transition: all 0.2s;
}
.btn-outline:hover { background: var(--teal); color: #fff; }

/* Section labels */
.section-label {
  font-size: 0.7rem; letter-spacing: 0.2em; text-transform: uppercase;
  color: var(--gold); font-weight: 500; margin-bottom: 12px;
  display: block;
}
```

- [ ] **Step 6: Create child theme functions.php**

Create `wp-content/themes/generatepress-child/functions.php`:
```php
<?php
add_action('wp_enqueue_scripts', function() {
    wp_enqueue_style('parent-style', get_template_directory_uri() . '/style.css');
});
```

- [ ] **Step 7: Activate child theme**

Appearance → Themes → activate "Haven Tile Child".

- [ ] **Step 8: Commit**

```bash
git add wp-content/themes/generatepress-child/
git commit -m "feat: add child theme with design tokens and base CSS"
```

---

## Phase 3 — Homepage Build

### Task 4: Build Homepage Hero Section

**Files:**
- Create: `wp-content/themes/generatepress-child/template-homepage.php`
- Modify: `wp-content/themes/generatepress-child/style.css`

- [ ] **Step 1: Create homepage template file**

Create `wp-content/themes/generatepress-child/template-homepage.php`:
```php
<?php
/*
 * Template Name: Homepage
 */
get_header(); ?>

<section class="hero">
  <div class="hero-bg" style="background-image: url('<?php echo get_field('hero_background_image'); ?>')"></div>
  <div class="hero-overlay"></div>
  <div class="hero-content">
    <span class="hero-eyebrow"><?php echo get_field('hero_eyebrow'); ?></span>
    <h1><?php echo get_field('hero_headline'); ?></h1>
    <p class="hero-sub"><?php echo get_field('hero_subtext'); ?></p>
    <div class="hero-actions">
      <a href="<?php echo get_field('hero_cta_1_url'); ?>" class="btn-primary"><?php echo get_field('hero_cta_1_text'); ?></a>
      <a href="<?php echo get_field('hero_cta_2_url'); ?>" class="btn-outline"><?php echo get_field('hero_cta_2_text'); ?></a>
    </div>
  </div>
</section>

<?php get_footer(); ?>
```

- [ ] **Step 2: Add hero CSS to style.css**

Append to `wp-content/themes/generatepress-child/style.css`:
```css
.hero {
  position: relative; height: 100vh; min-height: 680px;
  display: flex; align-items: center; overflow: hidden;
}
.hero-bg {
  position: absolute; inset: 0;
  background-size: cover; background-position: center;
}
.hero-overlay {
  position: absolute; inset: 0;
  background: linear-gradient(105deg, rgba(253,250,245,0.88) 40%, rgba(253,250,245,0.3) 100%);
}
.hero-content {
  position: relative; z-index: 2;
  padding: 0 80px; max-width: 700px;
}
.hero-eyebrow {
  display: inline-block; font-size: 0.72rem;
  letter-spacing: 0.18em; text-transform: uppercase;
  color: var(--gold); font-weight: 500; margin-bottom: 20px;
  border-bottom: 1px solid var(--gold); padding-bottom: 6px;
}
.hero h1 {
  font-size: clamp(2.6rem, 5vw, 4.2rem);
  line-height: 1.12; color: var(--teal); margin-bottom: 22px;
}
.hero h1 em { font-style: italic; color: var(--charcoal); }
.hero-sub {
  font-size: 1.05rem; line-height: 1.7; color: var(--gray);
  max-width: 480px; margin-bottom: 40px; font-weight: 300;
}
.hero-actions { display: flex; gap: 16px; flex-wrap: wrap; }
```

- [ ] **Step 3: Register ACF fields for hero**

In WP Admin → Custom Fields → Add New → Field Group name: "Homepage Hero" → add fields:
- `hero_background_image` (Image, return URL)
- `hero_eyebrow` (Text)
- `hero_headline` (Text)
- `hero_subtext` (Textarea)
- `hero_cta_1_text` (Text)
- `hero_cta_1_url` (URL)
- `hero_cta_2_text` (Text)
- `hero_cta_2_url` (URL)

Set Location: show if Page Template = Homepage. Publish.

- [ ] **Step 4: Create homepage page and assign template**

Pages → Add New → title: "Home" → Page Attributes → Template: Homepage → fill in ACF fields with hero content from mockup → Publish.

- [ ] **Step 5: Set as front page**

Settings → Reading → "A static page" → Front page: Home → Save.

- [ ] **Step 6: Verify hero renders correctly**

Visit `http://haven-tile-stone.local` — confirm hero image, headline, and both CTAs render with correct fonts and colors.

- [ ] **Step 7: Commit**

```bash
git add wp-content/themes/generatepress-child/
git commit -m "feat: homepage hero section with ACF fields"
```

---

### Task 5: Build Trust Bar, Services Grid, About Section

**Files:**
- Modify: `wp-content/themes/generatepress-child/template-homepage.php`
- Modify: `wp-content/themes/generatepress-child/style.css`

- [ ] **Step 1: Add trust bar HTML to homepage template**

After the hero section, add to `template-homepage.php`:
```php
<div class="trust-bar">
  <div class="trust-item"><div class="dot"></div> Licensed &amp; Insured in North Carolina</div>
  <div class="trust-item"><div class="dot"></div> 15+ Years of Coastal Experience</div>
  <div class="trust-item"><div class="dot"></div> High-End Residential Specialists</div>
  <div class="trust-item"><div class="dot"></div> 5-Star Rated on Google</div>
</div>
```

- [ ] **Step 2: Add trust bar CSS**

```css
.trust-bar {
  background: var(--teal); padding: 20px 60px;
  display: flex; justify-content: center; gap: 60px; flex-wrap: wrap;
}
.trust-item {
  display: flex; align-items: center; gap: 10px;
  color: rgba(255,255,255,0.85); font-size: 0.82rem; letter-spacing: 0.04em;
}
.trust-item .dot { width: 5px; height: 5px; background: var(--gold); border-radius: 50%; }
```

- [ ] **Step 3: Add services grid to homepage template**

```php
<section class="services" id="services">
  <div class="section-header">
    <span class="section-label">What We Do</span>
    <h2>Crafted for <em>Every Space</em></h2>
    <p class="section-sub">From spa-inspired master baths to showstopping kitchens — Haven Tile &amp; Stone brings precision and artistry to every project.</p>
  </div>
  <div class="services-grid">
    <?php
    $services = [
      ['num'=>'01','name'=>'Bathroom Remodeling','desc'=>'Custom shower surrounds, heated floors, and full bath transformations.','img'=>'https://images.unsplash.com/photo-1552321554-5fefe8c9ef14?w=600&q=80','url'=>'/bathroom-remodeling'],
      ['num'=>'02','name'=>'Kitchen Remodeling','desc'=>'Statement backsplashes, porcelain countertops, and full kitchen tile.','img'=>'https://images.unsplash.com/photo-1556909114-f6e7ad7d3136?w=600&q=80','url'=>'/kitchen-remodeling'],
      ['num'=>'03','name'=>'Outdoor & Pool Decks','desc'=>'Pool surrounds, coastal patios, and outdoor entertaining spaces.','img'=>'https://images.unsplash.com/photo-1575517111839-3a3843ee7f5d?w=600&q=80','url'=>'/outdoor-pool-decks'],
      ['num'=>'04','name'=>'Whole-Home Flooring','desc'=>'Large-format porcelain, natural stone, luxury tile flooring.','img'=>'https://images.unsplash.com/photo-1586105251261-72a756497a11?w=600&q=80','url'=>'/whole-home-flooring'],
    ];
    foreach($services as $s): ?>
    <a href="<?php echo $s['url']; ?>" class="service-card">
      <img src="<?php echo $s['img']; ?>" alt="<?php echo $s['name']; ?> Ocean Isle Beach NC">
      <div class="service-overlay">
        <div class="service-num"><?php echo $s['num']; ?></div>
        <div class="service-name"><?php echo $s['name']; ?></div>
        <div class="service-desc"><?php echo $s['desc']; ?></div>
        <div class="service-link">Explore service</div>
      </div>
    </a>
    <?php endforeach; ?>
  </div>
</section>
```

- [ ] **Step 4: Add services CSS**

```css
.services { background: var(--sand); padding: 100px 60px; }
.section-header { margin-bottom: 60px; }
.services-grid { display: grid; grid-template-columns: repeat(4,1fr); gap: 2px; }
.service-card { position: relative; overflow: hidden; height: 480px; display: block; text-decoration: none; }
.service-card img { width:100%; height:100%; object-fit:cover; transition: transform 0.6s; }
.service-card:hover img { transform: scale(1.05); }
.service-overlay {
  position: absolute; inset: 0;
  background: linear-gradient(to top, rgba(26,26,26,0.85) 0%, transparent 60%);
  display: flex; flex-direction: column; justify-content: flex-end;
  padding: 32px 28px;
}
.service-card:hover .service-overlay { background: linear-gradient(to top, rgba(42,63,60,0.92) 0%, rgba(42,63,60,0.3) 60%); }
.service-num { font-size: 0.65rem; letter-spacing: 0.18em; text-transform: uppercase; color: var(--gold); margin-bottom: 8px; }
.service-name { font-family: 'Playfair Display',serif; font-size: 1.5rem; color: #fff; margin-bottom: 10px; }
.service-desc { font-size: 0.82rem; color: rgba(255,255,255,0.75); line-height: 1.6; margin-bottom: 18px; }
.service-link { font-size: 0.72rem; letter-spacing: 0.14em; text-transform: uppercase; color: var(--gold); }
.service-link::after { content: ' →'; }
```

- [ ] **Step 5: Add about/Donovan section to template**

```php
<section class="about" id="about">
  <div class="about-img">
    <img src="<?php echo get_field('about_photo'); ?>" alt="Donovan - Haven Tile and Stone owner Ocean Isle Beach">
    <div class="about-badge">
      <div class="num">15+</div>
      <div class="label">Years on the Carolina Coast</div>
    </div>
  </div>
  <div class="about-content">
    <span class="section-label">Meet Donovan</span>
    <h2>Built on <em>Craft, Not Corners</em></h2>
    <div class="about-body"><?php echo get_field('about_body'); ?></div>
    <div class="about-stats">
      <div class="stat"><div class="num">350+</div><div class="label">Projects Completed</div></div>
      <div class="stat"><div class="num">100%</div><div class="label">Client Satisfaction</div></div>
      <div class="stat"><div class="num">3</div><div class="label">Counties Served</div></div>
    </div>
    <a href="#contact" class="btn-primary">Start Your Project</a>
  </div>
</section>
```

- [ ] **Step 6: Add about CSS to style.css**

```css
.about {
  display: grid; grid-template-columns: 1fr 1fr;
  gap: 80px; align-items: center; padding: 100px 60px;
}
.about-img { position: relative; }
.about-img img { width:100%; height:580px; object-fit:cover; }
.about-badge {
  position: absolute; bottom: -24px; right: -24px;
  width: 160px; height: 160px; background: var(--gold);
  display: flex; flex-direction: column; align-items: center; justify-content: center; text-align: center;
}
.about-badge .num { font-family: 'Playfair Display',serif; font-size: 2.8rem; color:#fff; line-height:1; }
.about-badge .label { font-size: 0.65rem; letter-spacing: 0.12em; text-transform: uppercase; color: rgba(255,255,255,0.85); margin-top: 4px; }
.about-stats { display: flex; gap: 40px; margin: 28px 0 36px; }
.stat .num { font-family: 'Playfair Display',serif; font-size: 2.2rem; color: var(--teal); }
.stat .label { font-size: 0.78rem; color: var(--gray); margin-top: 2px; }
```

- [ ] **Step 7: Verify homepage mid-sections render**

Visit `http://haven-tile-stone.local` — confirm trust bar, services grid with hover effects, and about section all render correctly.

- [ ] **Step 8: Commit**

```bash
git add wp-content/themes/generatepress-child/
git commit -m "feat: trust bar, services grid, and about section"
```

---

### Task 6: Build Portfolio, Testimonials, CTA Banner, Contact Form, Footer

**Files:**
- Modify: `wp-content/themes/generatepress-child/template-homepage.php`
- Modify: `wp-content/themes/generatepress-child/style.css`
- Create: `wp-content/themes/generatepress-child/template-parts/contact-form.php`

- [ ] **Step 1: Create the WPForms lead capture form**

WP Admin → WPForms → Add New → "Simple Contact Form" template → rename to "Quote Request Form" → add/reorder fields:
- First Name (required)
- Last Name (required)
- Phone (required)
- Email (required)
- Service Needed (Dropdown: Bathroom Remodeling, Kitchen Remodeling, Outdoor/Pool Deck, Whole-Home Flooring, Multiple/Not Sure)
- Your Location (Dropdown: Ocean Isle Beach, Holden Beach, Sunset Beach, Brunswick County, Myrtle Beach Area, Wilmington/Leland, Other)
- Tell Us About Your Project (Paragraph Text)

Save. Note the form shortcode (e.g., `[wpforms id="5"]`).

- [ ] **Step 2: Configure form notifications**

WPForms → Edit form → Settings → Notifications → set "Send To Email Address" to Donovan's email → set Subject: "New Quote Request — {field_id="5"} in {field_id="6"}" → Save.

- [ ] **Step 3: Add portfolio, testimonials, CTA, and contact sections to homepage template**

Add after about section in `template-homepage.php`:

```php
<!-- PORTFOLIO TEASER -->
<section class="portfolio" id="portfolio">
  <div class="section-header">
    <span class="section-label">Our Work</span>
    <h2>Recent <em>Projects</em></h2>
    <p class="section-sub">Every project is a reflection of our commitment to quality.</p>
  </div>
  <div class="portfolio-grid">
    <!-- Portfolio images pulled from ACF repeater field 'portfolio_items' -->
    <?php if(have_rows('portfolio_items')): while(have_rows('portfolio_items')): the_row();
      $img = get_sub_field('image'); $label = get_sub_field('label'); ?>
    <div class="portfolio-item">
      <img src="<?php echo $img; ?>" alt="<?php echo $label; ?>">
      <div class="portfolio-item-overlay"><div class="portfolio-item-label"><?php echo $label; ?></div></div>
    </div>
    <?php endwhile; endif; ?>
  </div>
  <div class="portfolio-footer"><a href="/portfolio" class="btn-outline">View Full Portfolio</a></div>
</section>

<!-- TESTIMONIALS -->
<section class="testimonials">
  <span class="section-label">Client Stories</span>
  <h2>What Homeowners <em>Say</em></h2>
  <div class="testimonials-grid">
    <?php if(have_rows('testimonials')): while(have_rows('testimonials')): the_row(); ?>
    <div class="testimonial-card">
      <div class="stars">★★★★★</div>
      <div class="testimonial-text">"<?php echo get_sub_field('quote'); ?>"</div>
      <div class="testimonial-author"><?php echo get_sub_field('author'); ?></div>
      <div class="testimonial-location"><?php echo get_sub_field('location'); ?></div>
    </div>
    <?php endwhile; endif; ?>
  </div>
</section>

<!-- CTA BANNER -->
<section class="cta-banner">
  <div class="cta-banner-bg" style="background-image:url('https://images.unsplash.com/photo-1556909114-f6e7ad7d3136?w=1600&q=80')"></div>
  <div class="cta-banner-overlay"></div>
  <div class="cta-banner-content">
    <span class="section-label">Ready to Start?</span>
    <h2>Let's Build <em>Something Beautiful</em></h2>
    <p>Whether you're renovating a beach house or upgrading your outdoor living space — Haven Tile &amp; Stone is ready to bring your vision to life.</p>
    <div class="cta-actions">
      <a href="#contact" class="btn-primary">Request a Free Quote</a>
      <a href="/contact" class="btn-outline">Schedule a Consultation</a>
    </div>
  </div>
  <div class="cta-contact">
    <a href="tel:<?php echo get_field('phone_number', 'option'); ?>" class="phone"><?php echo get_field('phone_display', 'option'); ?></a>
    <div class="cta-hours">Mon–Fri 8am–6pm · Sat by Appointment</div>
  </div>
</section>

<!-- CONTACT FORM -->
<section class="contact-section" id="contact">
  <div class="contact-info">
    <span class="section-label">Get in Touch</span>
    <h2>Start Your <em>Project Today</em></h2>
    <p>Tell us about your project and we'll follow up within 24 hours to schedule your free in-home consultation.</p>
    <div class="contact-detail"><span class="icon">📞</span><div><div class="info-label">Phone / Text</div><div class="info-value"><?php echo get_field('phone_display','option'); ?></div></div></div>
    <div class="contact-detail"><span class="icon">✉️</span><div><div class="info-label">Email</div><div class="info-value"><?php echo get_field('email','option'); ?></div></div></div>
    <div class="contact-detail"><span class="icon">📍</span><div><div class="info-label">Service Area</div><div class="info-value">Ocean Isle Beach &amp; Brunswick County, NC</div></div></div>
  </div>
  <div class="form-wrapper">
    [wpforms id="5"]
  </div>
</section>
```

- [ ] **Step 4: Add remaining CSS for portfolio, testimonials, cta, contact**

```css
/* Portfolio */
.portfolio { background: var(--sand); padding: 100px 60px; }
.portfolio-grid { display: grid; grid-template-columns: repeat(3,1fr); grid-template-rows: 280px 280px; gap: 3px; }
.portfolio-item { overflow: hidden; position: relative; }
.portfolio-item:first-child { grid-row: 1/3; }
.portfolio-item img { width:100%; height:100%; object-fit:cover; transition: transform 0.5s; }
.portfolio-item:hover img { transform: scale(1.04); }
.portfolio-item-overlay { position:absolute; inset:0; background: rgba(42,63,60,0); display:flex; align-items:center; justify-content:center; transition: background 0.3s; }
.portfolio-item:hover .portfolio-item-overlay { background: rgba(42,63,60,0.5); }
.portfolio-item-label { color:#fff; font-family:'Playfair Display',serif; font-size:1.1rem; font-style:italic; opacity:0; transition:opacity 0.3s; }
.portfolio-item:hover .portfolio-item-label { opacity:1; }
.portfolio-footer { display:flex; justify-content:center; margin-top:48px; }

/* Testimonials */
.testimonials { background: var(--teal); padding: 100px 60px; }
.testimonials .section-label { color: var(--gold); }
.testimonials h2 { color: #fff; }
.testimonials-grid { display: grid; grid-template-columns: repeat(3,1fr); gap: 32px; margin-top: 60px; }
.testimonial-card { background: rgba(255,255,255,0.06); border: 1px solid rgba(255,255,255,0.1); padding: 36px 32px; }
.stars { color: var(--gold); font-size: 0.9rem; letter-spacing: 2px; margin-bottom: 18px; }
.testimonial-text { font-family:'Playfair Display',serif; font-size:1.05rem; font-style:italic; color:rgba(255,255,255,0.9); line-height:1.7; margin-bottom:24px; }
.testimonial-author { font-size:0.78rem; color: var(--gold-lt,#D9BF8A); letter-spacing:0.08em; text-transform:uppercase; }
.testimonial-location { font-size:0.72rem; color:rgba(255,255,255,0.4); margin-top:4px; }

/* CTA Banner */
.cta-banner { position:relative; padding:100px 60px; display:flex; align-items:center; justify-content:space-between; gap:48px; }
.cta-banner-bg { position:absolute; inset:0; background-size:cover; background-position:center; }
.cta-banner-overlay { position:absolute; inset:0; background: linear-gradient(90deg, rgba(245,240,232,0.97) 45%, rgba(245,240,232,0.6) 100%); }
.cta-banner-content, .cta-contact { position:relative; z-index:2; }
.cta-contact .phone { font-family:'Playfair Display',serif; font-size:2rem; color:var(--teal); text-decoration:none; display:block; margin-bottom:8px; }
.cta-hours { font-size:0.78rem; color:var(--gray); }

/* Contact */
.contact-section { background: var(--sand); padding:100px 60px; display:grid; grid-template-columns:1fr 1fr; gap:80px; align-items:start; }
.contact-detail { display:flex; gap:14px; align-items:flex-start; margin-bottom:20px; }
.info-label { font-size:0.7rem; letter-spacing:0.1em; text-transform:uppercase; color:var(--gray); }
.info-value { font-size:0.95rem; color:var(--teal); font-weight:500; }
```

- [ ] **Step 5: Verify complete homepage**

Visit `http://haven-tile-stone.local` — scroll through entire page confirming all sections render correctly. Test form submission in a private window.

- [ ] **Step 6: Commit**

```bash
git add wp-content/themes/generatepress-child/
git commit -m "feat: complete homepage — portfolio, testimonials, cta, contact form"
```

---

## Phase 4 — Service Pages

### Task 7: Create Service Page Template

**Files:**
- Create: `wp-content/themes/generatepress-child/template-service.php`
- Modify: `wp-content/themes/generatepress-child/style.css`

- [ ] **Step 1: Create service page template**

Create `wp-content/themes/generatepress-child/template-service.php`:
```php
<?php /* Template Name: Service Page */ get_header(); ?>

<section class="service-hero">
  <div class="hero-bg" style="background-image:url('<?php echo get_field('service_hero_image'); ?>')"></div>
  <div class="hero-overlay"></div>
  <div class="hero-content">
    <span class="hero-eyebrow"><?php echo get_field('service_area_label'); ?></span>
    <h1><?php the_title(); ?></h1>
    <p class="hero-sub"><?php echo get_field('service_hero_subtext'); ?></p>
    <a href="#contact" class="btn-primary">Request a Free Quote</a>
  </div>
</section>

<section class="service-body">
  <div class="service-content">
    <span class="section-label"><?php echo get_field('service_label'); ?></span>
    <h2><?php echo get_field('service_section_headline'); ?></h2>
    <div class="service-text"><?php echo get_field('service_description'); ?></div>
    <ul class="service-features">
      <?php if(have_rows('service_features')): while(have_rows('service_features')): the_row(); ?>
      <li><?php echo get_sub_field('feature'); ?></li>
      <?php endwhile; endif; ?>
    </ul>
  </div>
  <div class="service-gallery">
    <?php if(have_rows('service_gallery_images')): while(have_rows('service_gallery_images')): the_row(); ?>
    <img src="<?php echo get_sub_field('image'); ?>" alt="<?php the_title(); ?> <?php echo get_sub_field('caption'); ?>">
    <?php endwhile; endif; ?>
  </div>
</section>

<section class="service-locations-cta">
  <h2>Serving <em>Your Area</em></h2>
  <p>Haven Tile &amp; Stone provides <?php the_title(); ?> services across Ocean Isle Beach, Holden Beach, Sunset Beach, Brunswick County, and the Myrtle Beach area.</p>
  <div class="location-grid">
    <a href="/bathroom-remodeling-ocean-isle-beach-nc" class="location-chip">📍 Ocean Isle Beach</a>
    <a href="/bathroom-remodeling-holden-beach-nc" class="location-chip">📍 Holden Beach</a>
    <a href="/bathroom-remodeling-sunset-beach-nc" class="location-chip">📍 Sunset Beach</a>
    <a href="/bathroom-remodeling-brunswick-county-nc" class="location-chip">📍 Brunswick County</a>
  </div>
</section>

<?php get_template_part('template-parts/contact-form'); ?>
<?php get_footer(); ?>
```

- [ ] **Step 2: Create the 4 service pages in WP Admin**

Pages → Add New for each:
1. Title: "Bathroom Remodeling" → Template: Service Page → slug: `bathroom-remodeling` → fill ACF fields → Publish
2. Title: "Kitchen Remodeling" → Template: Service Page → slug: `kitchen-remodeling` → fill ACF fields → Publish
3. Title: "Outdoor & Pool Decks" → Template: Service Page → slug: `outdoor-pool-decks` → fill ACF fields → Publish
4. Title: "Whole-Home Flooring" → Template: Service Page → slug: `whole-home-flooring` → fill ACF fields → Publish

- [ ] **Step 3: Add service page CSS**

```css
.service-body { display: grid; grid-template-columns: 1fr 1fr; gap: 80px; padding: 100px 60px; align-items: start; }
.service-features { list-style: none; margin-top: 24px; }
.service-features li { padding: 10px 0; border-bottom: 1px solid var(--border); font-size: 0.95rem; color: var(--gray); }
.service-features li::before { content: '—  '; color: var(--gold); }
.service-gallery { display: grid; grid-template-columns: 1fr 1fr; gap: 4px; }
.service-gallery img { width: 100%; height: 240px; object-fit: cover; }
```

- [ ] **Step 4: Verify all 4 service pages**

Visit each URL: `/bathroom-remodeling`, `/kitchen-remodeling`, `/outdoor-pool-decks`, `/whole-home-flooring` — confirm content and template render correctly.

- [ ] **Step 5: Commit**

```bash
git add wp-content/themes/generatepress-child/
git commit -m "feat: service page template and 4 service pages"
```

---

## Phase 5 — Location Landing Pages

### Task 8: Create Location Page Template and Primary Location Pages

**Files:**
- Create: `wp-content/themes/generatepress-child/template-location.php`

- [ ] **Step 1: Create location page template**

Create `wp-content/themes/generatepress-child/template-location.php`:
```php
<?php /* Template Name: Location Page */ get_header(); ?>

<section class="location-hero">
  <div class="hero-bg" style="background-image:url('<?php echo get_field('location_hero_image'); ?>')"></div>
  <div class="hero-overlay"></div>
  <div class="hero-content">
    <span class="hero-eyebrow">Tile &amp; Stone Installation</span>
    <h1><?php the_title(); ?></h1>
    <p class="hero-sub"><?php echo get_field('location_hero_subtext'); ?></p>
    <a href="#contact" class="btn-primary">Get a Free Quote</a>
  </div>
</section>

<section class="location-intro" style="padding:80px 60px; max-width:800px; margin:0 auto;">
  <span class="section-label"><?php echo get_field('location_name'); ?></span>
  <h2><?php echo get_field('location_headline'); ?></h2>
  <div><?php echo get_field('location_body'); ?></div>
</section>

<section class="location-services" style="padding:0 60px 80px;">
  <h2>Our Services in <em><?php echo get_field('location_name'); ?></em></h2>
  <div class="services-grid">
    <a href="/bathroom-remodeling" class="service-card"><!-- same service card markup --></a>
    <a href="/kitchen-remodeling" class="service-card"><!-- ... --></a>
    <a href="/outdoor-pool-decks" class="service-card"><!-- ... --></a>
    <a href="/whole-home-flooring" class="service-card"><!-- ... --></a>
  </div>
</section>

<?php get_template_part('template-parts/contact-form'); ?>
<?php get_footer(); ?>
```

- [ ] **Step 2: Create primary location pages**

Pages → Add New for each location:
1. "Tile Contractor Ocean Isle Beach NC" → slug: `tile-contractor-ocean-isle-beach-nc` → Template: Location Page → Publish
2. "Tile Contractor Holden Beach NC" → slug: `tile-contractor-holden-beach-nc` → Publish
3. "Tile Contractor Sunset Beach NC" → slug: `tile-contractor-sunset-beach-nc` → Publish
4. "Tile Contractor Brunswick County NC" → slug: `tile-contractor-brunswick-county-nc` → Publish
5. "Tile Contractor Myrtle Beach SC" → slug: `tile-contractor-myrtle-beach-sc` → Publish

Write unique, location-specific body copy for each (150+ words minimum, mention local landmarks, neighborhoods).

- [ ] **Step 3: Verify location pages**

Visit each location page URL — confirm hero, intro, services grid, and contact form render. Check no duplicate content between pages.

- [ ] **Step 4: Commit**

```bash
git add wp-content/themes/generatepress-child/
git commit -m "feat: location page template and 5 primary location pages"
```

---

## Phase 6 — SEO Configuration

### Task 9: Configure RankMath for Local SEO

**Files:**
- WP Admin → RankMath → Settings

- [ ] **Step 1: Configure local business schema**

RankMath → Titles & Meta → Local SEO:
- Business Name: Haven Tile & Stone
- Business Type: Home Improvement
- Address: (Donovan's business address or Ocean Isle Beach, NC)
- Phone: Donovan's number
- Opening Hours: Mon–Fri 8am–6pm, Sat by Appointment

- [ ] **Step 2: Set homepage meta**

Edit Home page → RankMath panel:
- SEO Title: `Luxury Tile & Stone Installation | Ocean Isle Beach NC | Haven Tile & Stone`
- Meta Description: `Haven Tile & Stone delivers luxury bathroom, kitchen, and outdoor tile installation across Ocean Isle Beach, Holden Beach, and Brunswick County, NC. Request your free quote today.`

- [ ] **Step 3: Set meta for each service page**

Bathroom page:
- Title: `Bathroom Tile Remodeling Ocean Isle Beach NC | Haven Tile & Stone`
- Description: `Transform your bathroom with custom tile, heated floors & shower surrounds. Haven Tile & Stone serves Ocean Isle Beach, Holden Beach & Brunswick County. Call for a free quote.`

Kitchen page:
- Title: `Kitchen Tile & Backsplash Installation Brunswick County NC | Haven Tile & Stone`
- Description: `Stunning kitchen backsplashes, countertop tile & full kitchen tile installations. Serving Ocean Isle Beach, Holden Beach & Sunset Beach, NC. Free consultations available.`

Outdoor page:
- Title: `Pool Deck & Outdoor Tile Installation Ocean Isle Beach NC | Haven Tile & Stone`
- Description: `Pool surrounds, coastal patios & outdoor tile built for Carolina salt air. Haven Tile & Stone serves Brunswick County, NC. Request your free estimate today.`

Flooring page:
- Title: `Luxury Tile Flooring Installation Brunswick County NC | Haven Tile & Stone`
- Description: `Large-format porcelain & natural stone flooring installed with precision. Serving Ocean Isle Beach, Holden Beach, Sunset Beach & the Myrtle Beach area.`

- [ ] **Step 4: Set meta for each location page**

Ocean Isle page:
- Title: `Tile Contractor Ocean Isle Beach NC | Haven Tile & Stone`
- Description: `Haven Tile & Stone is Ocean Isle Beach's trusted luxury tile contractor. Bathrooms, kitchens, pool decks & flooring. Call (910) XXX-XXXX for a free quote.`

(Repeat pattern for each location — unique title and description for every page.)

- [ ] **Step 5: Enable XML sitemap**

RankMath → Sitemap → enable sitemap → include all pages, posts. Submit URL (`/sitemap_index.xml`) to Google Search Console.

- [ ] **Step 6: Verify no duplicate meta across pages**

Open each page in a new tab → right-click → View Page Source → search for `<title>` → confirm every page has a unique title tag.

- [ ] **Step 7: Commit**

```bash
git commit -m "feat: rankmath local SEO config, meta titles, and schema"
```

---

## Phase 7 — Portfolio Page

### Task 10: Build Portfolio / Gallery Page

**Files:**
- Create: `wp-content/themes/generatepress-child/template-portfolio.php`

- [ ] **Step 1: Create Portfolio custom post type**

Add to `wp-content/themes/generatepress-child/functions.php`:
```php
add_action('init', function() {
    register_post_type('portfolio', [
        'labels' => ['name' => 'Portfolio', 'singular_name' => 'Project'],
        'public' => true,
        'has_archive' => false,
        'supports' => ['title', 'thumbnail', 'excerpt'],
        'menu_icon' => 'dashicons-format-image',
        'show_in_rest' => true,
    ]);
});
```

- [ ] **Step 2: Add portfolio ACF fields**

Custom Fields → Add New → "Portfolio Project" fields:
- `project_category` (Select: Bathroom, Kitchen, Outdoor, Flooring)
- `project_location` (Text — e.g., "Ocean Isle Beach, NC")
- `project_gallery` (Gallery)

Location: Post Type = portfolio. Publish.

- [ ] **Step 3: Add 6–10 portfolio projects**

Portfolio (custom post type) → Add New for each project → add featured image, title, category, location → Publish.

- [ ] **Step 4: Create portfolio template**

Create `wp-content/themes/generatepress-child/template-portfolio.php`:
```php
<?php /* Template Name: Portfolio */ get_header(); ?>
<section style="padding:140px 60px 60px;">
  <span class="section-label">Our Work</span>
  <h1>Project <em>Gallery</em></h1>
  <p>Browse our portfolio of luxury tile and stone installations across the Carolina coast.</p>
</section>

<section style="padding:0 60px 100px;">
  <div class="portfolio-full-grid">
    <?php
    $projects = new WP_Query(['post_type'=>'portfolio','posts_per_page'=>-1]);
    while($projects->have_posts()): $projects->the_post(); ?>
    <div class="portfolio-item">
      <?php the_post_thumbnail('large'); ?>
      <div class="portfolio-item-overlay">
        <div class="portfolio-item-label"><?php the_title(); ?></div>
      </div>
    </div>
    <?php endwhile; wp_reset_postdata(); ?>
  </div>
</section>
<?php get_template_part('template-parts/contact-form'); get_footer(); ?>
```

- [ ] **Step 5: Add portfolio full grid CSS**

```css
.portfolio-full-grid { display: grid; grid-template-columns: repeat(3,1fr); gap: 4px; }
.portfolio-full-grid .portfolio-item { height: 320px; }
```

- [ ] **Step 6: Create portfolio page and set template**

Pages → Add New → "Portfolio" → Template: Portfolio → Publish. Add to nav menu.

- [ ] **Step 7: Commit**

```bash
git add wp-content/themes/generatepress-child/
git commit -m "feat: portfolio CPT and gallery page"
```

---

## Phase 8 — Performance & Launch Prep

### Task 11: Performance, Cloudflare, and Pre-Launch Checklist

- [ ] **Step 1: Configure WP Rocket / caching plugin**

WP Rocket → Settings → enable: Page Cache, Browser Cache, GZIP Compression, LazyLoad images, Minify CSS/JS.

- [ ] **Step 2: Add Cloudflare (when ready to go live)**

Log into Cloudflare → Add site → enter haventileandstone.com → select Free plan → update nameservers at GoDaddy to Cloudflare's nameservers → enable "Always Use HTTPS" → enable "Auto Minify" (HTML, CSS, JS).

- [ ] **Step 3: Run pre-launch checklist**

- [ ] All pages have unique title tags (check View Source)
- [ ] All images have alt text
- [ ] Contact form sends email to Donovan — test with real submission
- [ ] Phone number is clickable (tel: link) on mobile
- [ ] All internal links work (no 404s) — install "Broken Link Checker" plugin temporarily
- [ ] Google Analytics / GA4 installed — add tracking ID in RankMath → Analytics
- [ ] Google Search Console verified — submit sitemap
- [ ] SSL certificate active (https:// not http://)
- [ ] Site loads under 3 seconds — test at pagespeed.web.dev
- [ ] Mobile responsive — test at search.google.com/test/mobile-friendly
- [ ] Favicon set — Appearance → Customize → Site Identity

- [ ] **Step 4: Point GoDaddy domain to new host (when ready)**

In GoDaddy → DNS → change nameservers to Cloudflare nameservers (provided in Step 2). DNS propagates in 0–48 hours.

- [ ] **Step 5: Final commit**

```bash
git add .
git commit -m "chore: pre-launch performance config and checklist complete"
```

---

## Ongoing SEO Campaign Tasks (Post-Launch)

### Task 12: Ongoing SEO — Monthly Cadence

- [ ] **Week 1 each month:** Publish 1 blog post targeting a long-tail keyword (e.g., "best tile for coastal homes Carolina", "how to choose tile for a pool deck NC")
- [ ] **Week 2:** Add one new location × service landing page (e.g., `/bathroom-tile-ocean-isle-beach-nc`)
- [ ] **Week 3:** Post 2–3 updates to Google Business Profile (photo of recent project + short caption)
- [ ] **Week 4:** Request a Google review from any recent client — send them a direct review link from GBP dashboard
- [ ] **Monthly:** Run Google Search Console → Performance → check which queries are getting impressions but low clicks → optimize those page titles/descriptions
- [ ] **Monthly:** Check Google Business Profile insights — calls, direction requests, profile views
