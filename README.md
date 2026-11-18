# Daysure website

Live at https://daysure.netlify.app

Waitlist site for Daysure, the daily spending allowance app. Plain HTML, no build step.

- `index.html`: the landing page with a live demo of the daily limit and two waitlist forms (email, or a mobile number with any country code, plus Android or iPhone).

The page works for every country. It guesses the visitor's country from their time zone and browser language, then shows amounts in that currency, picks their dialling code, and swaps India-only words (UPI, EMIs, GPay/PhonePe) for general ones. To preview another country, add `?country=` with a two-letter code, for example `/?country=GB` or `/?country=AE`.
- `thanks.html`: shown if someone submits the form with JavaScript turned off.
- `netlify.toml`: hosting settings.

## Going live (Netlify, free)

The waitlist form uses Netlify Forms, so sign-ups are stored without a server or database.

1. Sign in at https://app.netlify.com and choose **Add new site → Import an existing project → GitHub**, then pick `rohitsch1/pacing_website` (already done: the live project is `daysure` under the contact-rohitsch team, deployed with `npx netlify-cli deploy --prod --dir .`). Leave the build command empty and the publish directory as `.`.
2. In the new site, open **Forms** and click **Enable form detection**, then redeploy once (Deploys → Trigger deploy).
3. Sign-ups appear under **Forms → waitlist**. Each one has the email or mobile number (in international format, like +447400123456), the country, the phone type, and which form it came from (`hero` or `footer`). You can download them as CSV.
4. Optional: under **Forms → Form notifications**, add an email notification for each new sign-up.
5. Optional: connect your own domain under **Domain management**.

Opened from your own computer (localhost), the form shows the success message but saves nothing. It only stores sign-ups on the live Netlify site.

## Preview locally

```bash
python3 -m http.server 4321
```

Then open http://localhost:4321.

## Visitor tracking and conversion rate (Google Analytics 4)

The page is ready for GA4. It sends nothing until you add your Measurement ID.

1. Go to https://analytics.google.com, create an account and a **Web** property for your site address, and copy the **Measurement ID** (looks like `G-AB12CD34EF`).
2. In `index.html`, near the top, replace `G-XXXXXXXXXX` in `window.PACE_GA_ID = 'G-XXXXXXXXXX';` with your ID. Commit and push; Netlify redeploys on its own.
3. In GA4, open **Admin → Events** (or **Key events**) and mark `generate_lead` as a **key event**. That makes each waitlist sign-up count as a conversion.

What gets tracked:

| Event | When it fires |
| --- | --- |
| `page_view` | Every visit (automatic) |
| `cta_click` | The header "Join the waitlist" button is tapped |
| `demo_interact` | Someone drags the "spent today" slider |
| `waitlist_start` | Someone taps into the email or mobile box |
| `waitlist_invalid` | They try to submit a wrong email or number |
| `generate_lead` | A sign-up succeeds (with `method` email/phone, `country`, `device`, `form_location` hero/footer) |

**Conversion rate** = `generate_lead` count ÷ users. GA4 shows it under **Reports → Engagement → Key events** (the "Key event rate" column). For a step-by-step drop-off view, use **Explore → Funnel exploration** with the steps `page_view` → `waitlist_start` → `generate_lead`.

Privacy: visitors in the EU, EEA, UK and Switzerland see a short "Can we count your visit?" bar and are only counted if they tap Allow. Everyone else is counted by default and can opt out with **Analytics choices** in the footer. Ads features are always off. To test events on your own computer, open `http://localhost:4321/?debug_analytics=1` and watch GA4's **Admin → DebugView**.

## Motion and credits

- Animations: [GSAP](https://gsap.com) 3.13 with ScrollTrigger and SplitText, smooth scrolling by [Lenis](https://lenis.darkroom.engineering), all loaded from jsDelivr. Visitors who turn on "reduce motion" on their phone or computer get the page without animation.
- Sign-up confetti: "success confetti" by Deepesh Reddy from [LottieFiles](https://lottiefiles.com/free-animation/success-confetti-f5PdexvrBK), used under the Lottie Simple License, played with lottie-web.
- Font: Plus Jakarta Sans (Google Fonts).
