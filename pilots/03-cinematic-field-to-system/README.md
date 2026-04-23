# Pilot 03 — From Field to System

**סטטוס:** Shell deployed, ממתין ל-10 MP4s מ-Kie.ai (דורש reference image של אורי)
**URL:** https://simplifai.co.il/pilots/03-cinematic-field-to-system/
**Local:** `python -m http.server 8000 --directory projects/website` → http://localhost:8000/pilots/03-cinematic-field-to-system/

---

## הקונספט

המסע מתחיל בעולם הפיזי של עסקים ישראליים אמיתיים. תוך כדי גלילה, מעל כל סצנה פיזית עולה שכבה דיגיטלית — AI שמתחבר למציאות, לא מחליף אותה. בסוף חוזרים לאורי עצמו — אמיתי, במקום שלו.

**המסר:** *"AI לא מחליף את העסק שלך. הוא לובש אותו."*
**Hook ראשי:** `מהשטח. לא מהספר.`
**Target funnel:** MOFU (אמון + אמינות)

---

## Scene → Section Mapping

| Scene | Section | Title |
|---|---|---|
| 01 | Hero | מהשטח. לא מהספר. |
| 02 | Problem | 40 שעות בשבוע על עבודה שמחשב יכול |
| 03 | Current State | זה המצב שלך. מכיר? |
| 04 | Ecosystem | המערכות שלך — רק חסר חיבור |
| 05 | Audience | אתה אחד מאלה? |
| 06 | About Uri | 10 שנים בעסקים. אז AI. |
| 07 | Process | יום אחד. לפני ואחרי. |
| 08 | Before/After | 14 → 7 שעות. אותה הכנסה. |
| 09 | Packages | 3 גדלים. אותו עיקרון. |
| 10 | Final CTA | בוא נדבר. |

---

## DNA ויזואלי

| מימד | ערך |
|---|---|
| **Palette** | Warm amber `#1a1510` base (במקום pure navy) + electric blue `#4D8EFF` overlay + natural light tones |
| **Texture** | Documentary, natural light, 35mm grain, real textures (wood, paper, worn metal) |
| **Mood arc** | אותנטי → מהפכני → הישגי שקט |
| **Typography** | Heebo Bold לכותרות (דוקו-סטייל), Share Tech Mono ל-captions כמו title cards |

**הבדל מ-Pilot 02:** זה *לא* cinematic SaaS — זה *documentary-meets-AI*. הטמפרטורה חמה יותר, הטקסטורה יותר גולמית, הפונט הראשי משותק לטובת Heebo (שמרגיש ישראלי ואמיתי).

---

## ⚠️ דרישה מקדימה

**סצנות 06 + 10 דורשות reference image של אורי.**

- סצנה 06: over-the-shoulder של אורי בונה skill
- סצנה 10: פורטרט אורי במקום אותנטי

לפני הפקת הסצנות האלה ב-Kie.ai — תעלה תמונה טובה שלך (`uri-reference.png`, לא סטודיו, אור טבעי). אם אין — סצנות 01-05, 07-09 אפשר לייצר עכשיו ולחכות עם 06+10.

---

## Workflow (זהה ל-Pilot 02, עם הערה על Uri)

### Phase B (אורי)
1. ראה `image-prompts.md` — 10 פרומפטים
2. העלה `uri-reference.png` ל-Kie.ai (לסצנות 06+10)
3. ייצר 10 תמונות Nano Banana 2 (16:9)
4. אנמה כל אחת ב-Kling 2.0 (first=last frame)
5. הורד 10 MP4s ל-`assets/video-sources/`

### Phase C (Claude)
```bash
cd projects/website/pilots/03-cinematic-field-to-system
for i in 01 02 03 04 05 06 07 08 09 10; do
  ffmpeg -i assets/video-sources/scene-$i.mp4 \
    -vf "fps=1.33,scale=1920:1080" \
    -q:v 85 \
    assets/scroll-frames/s$i-%03d.webp
done
```

### Deploy
```bash
cp -r projects/website/pilots/03-cinematic-field-to-system ../simplifai-website/pilots/
cd ../simplifai-website
git add -A && git commit -m "pilot: 03 cinematic field-to-system" && git push
```

---

## Acceptance criteria

- [ ] 10 סקציות עם טקסט documentary
- [ ] Canvas background משתנה בגלילה (warm → cool)
- [ ] Hero headline "מהשטח. לא מהספר." ב-kinetic reveal
- [ ] סקציית About Uri (scene 06) בולטת — זה הלב של הפיילוט
- [ ] Mobile: reveals עובדים, tilt disabled
- [ ] `prefers-reduced-motion` → canvas סטטי
- [ ] Lighthouse mobile: Perf ≥ 70, A11y ≥ 95, SEO 100

---

## הבדלים מ-Pilot 02 (לסיכום)

| פרמטר | Pilot 02 (Digital Worker) | Pilot 03 (Field to System) |
|---|---|---|
| Hero hook | "בזמן שאתה ישן, הוא עובד" | "מהשטח. לא מהספר." |
| Nav items | בית / מערכת / שירותים / חבילות | בית / מי אני / תהליך / מסלולים |
| יש סקציית About? | לא (רק PROCESS) | **כן** (scene 06 — Uri) |
| Section 3 | WhatsApp Bot service | Current State recognition |
| Section 5 | 6 Services | 4 Audience personas |
| Packages | 3 tiers קלינסי | 3 tiers עם ציטוט לקוח בכל אחד |
| Background palette | pure navy → blue → sunrise | warm amber → blue → navy |
| Funnel fit | TOFU (קוסם) | MOFU (אמון) |
