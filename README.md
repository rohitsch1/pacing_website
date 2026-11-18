# Pace website

Waitlist site for Pace, the daily spending allowance app. Plain HTML, no build step.

- `index.html`: the landing page with a live demo of the daily limit and two waitlist forms (email, or a mobile number with any country code, plus Android or iPhone).

The page works for every country. It guesses the visitor's country from their time zone and browser language, then shows amounts in that currency, picks their dialling code, and swaps India-only words (UPI, EMIs, GPay/PhonePe) for general ones. To preview another country, add `?country=` with a two-letter code, for example `/?country=GB` or `/?country=AE`.
- `thanks.html`: shown if someone submits the form with JavaScript turned off.
- `netlify.toml`: hosting settings.

## Going live (Netlify, free)

The waitlist form uses Netlify Forms, so sign-ups are stored without a server or database.

1. Sign in at https://app.netlify.com and choose **Add new site → Import an existing project → GitHub**, then pick `rohitsch1/pacing_website`. Leave the build command empty and the publish directory as `.`.
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
