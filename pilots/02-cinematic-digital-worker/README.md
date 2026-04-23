# Pilot 02 — The Digital Worker Awakens

**סטטוס:** Shell deployed, ממתין ל-10 MP4s מ-Kie.ai
**URL (אחרי deploy):** https://simplifai.co.il/pilots/02-cinematic-digital-worker/
**Local:** `python -m http.server 8000 --directory projects/website` → http://localhost:8000/pilots/02-cinematic-digital-worker/

---

## הקונספט

משרד ריק בלילה. כל העמדות כבויות. מסך אחד נדלק — וממנו מתפשט אור שמפעיל בזה אחר זה כל מערכת בעסק. הזריחה עולה לאט. כשהבעלים נכנס בבוקר — הכל כבר עשוי.

**המסר:** *"בזמן שאתה ישן, העובד הדיגיטלי שלך עובד"*
**Hook ראשי:** `בזמן שאתה ישן, הוא עובד.`
**Target funnel:** TOFU + MOFU (קוסם + סקרן)

---

## Scene → Section Mapping

| Scene | Section | Hero/Title |
|---|---|---|
| 01 | Hero | בזמן שאתה ישן, הוא עובד. |
| 02 | Problem | 40% מהיום שלך הולך לפח |
| 03 | WhatsApp/Bot | 30 שניות לתשובה. 24/7. בעברית. |
| 04 | Ecosystem | המערכות שלך — מתחברות |
| 05 | Services | 6 עובדים דיגיטליים לבחירה |
| 06 | Process | 47 מערכות. שנה אחת. 80% דומה. |
| 07 | Before/After | 14 → 7 שעות. אותה הכנסה. |
| 08 | Packages | 3 מסלולים. בחר את שלך. |
| 09 | Testimonials | הזריחה של הלקוחות שלנו |
| 10 | Final CTA | הכל מוכן. רק תחליט. |

---

## DNA ויזואלי

| מימד | ערך |
|---|---|
| **Palette** | Deep navy `#060D1F` → electric blue `#4D8EFF` glows → sunrise orange `#FF6F4C` (בסוף) |
| **Texture** | Cinematic, long-exposure, grain דק, ערפילית כחולה קלה |
| **Mood arc** | שקט → התעוררות → שיא → רוגע הישגי |
| **Typography** | Outfit 900 (kinetic char reveal), Heebo body, Share Tech Mono labels |

---

## תלויות טכניות

- **Stack:** Vanilla HTML + inline CSS + vanilla JS
- **GSAP + ScrollTrigger** via cdnjs (`gsap 3.12.5`)
- **Canvas frame-scrubber** — custom `ScrollVideoBackground` class (see index.html script)
- **Fonts** — Outfit + Heebo + Share Tech Mono via Google Fonts (preload + onload swap, non-blocking)

## מבנה התיקייה

```
02-cinematic-digital-worker/
├── index.html               ← single-file pilot
├── README.md                ← זה הקובץ
├── image-prompts.md         ← 10 פרומפטים מוכנים ל-Kie.ai
└── assets/
    ├── video-sources/       ← MP4s מ-Kling (אורי מעלה לכאן)
    └── scroll-frames/       ← WebP frames שנחלצו מ-MP4 (Claude מייצר)
```

---

## Workflow

### Phase B (אורי)
ראה `image-prompts.md`. פלט: 10 קבצי `scene-01.mp4` עד `scene-10.mp4` ב-`assets/video-sources/`.

### Phase C (Claude — אחרי ש-MP4s זמינים)
```bash
cd projects/website/pilots/02-cinematic-digital-worker
for i in 01 02 03 04 05 06 07 08 09 10; do
  ffmpeg -i assets/video-sources/scene-$i.mp4 \
    -vf "fps=1.33,scale=1920:1080" \
    -q:v 85 \
    assets/scroll-frames/s$i-%03d.webp
done
```

מניב 20 פריימים לכל סצנה = 200 פריימים × ~70KB = **~14MB total**.

### Phase E — Local test
```bash
python -m http.server 8000 --directory projects/website
# פתיחה: http://localhost:8000/pilots/02-cinematic-digital-worker/
```

### Phase F — Deploy
```bash
cp -r projects/website/pilots/02-cinematic-digital-worker ../simplifai-website/pilots/
cd ../simplifai-website
git add -A && git commit -m "pilot: 02 cinematic digital-worker" && git push
# 60-90s rebuild → simplifai.co.il/pilots/02-cinematic-digital-worker/
```

---

## Fallback behavior (עד שיש frames)

עד ש-`assets/scroll-frames/*.webp` קיימים, ה-canvas מציג **CSS gradient fallback** (navy → electric blue → navy) שנראה טוב בפני עצמו. הכל שאר האתר (טיפוגרפיה, כרטיסים, אנימציות) עובד באופן מלא. זה מאפשר deploy של ה-shell מיד ולחוות את המבנה/CSS/אנימציות לפני שהתמונות מוכנות.

---

## Acceptance criteria

- [ ] 10 סקציות rendered עם הטקסט המעודכן
- [ ] Canvas background משתנה בגלילה (אחרי frames)
- [ ] Hero headline ב-kinetic char reveal (GSAP ScrollFloat)
- [ ] Panels נכנסים מלמטה + tilt על mousemove (desktop)
- [ ] Mobile — reveals עובדים, tilt disabled
- [ ] `prefers-reduced-motion: reduce` → canvas סטטי, אין אנימציות
- [ ] Pill nav עובד — scroll-to-section חלק
- [ ] Lighthouse mobile: Perf ≥ 70, A11y ≥ 95, SEO 100
