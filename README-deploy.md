# FreshAuthority — storefront deploy notes

## Contents
- `index.html` — the entire storefront (single file, inline CSS, no external assets, no JS). Premium single-page: hero with real dated volumes → mechanism → file contents (16 fields) → three-files-in-every-tier → state volume table → how it works → pricing → factoring section → free-sample form → FAQ → legal footer.

## Deploy
- Publish as a **subdomain on existing hosting** (e.g. `freshauthority.<existing-domain>` or its own domain when HQ assigns one). Pure static — drop `index.html` at the docroot. No build step, no server-side code.
- HQ handles the deploy itself (per the build brief).

## Before go-live — wires status (updated Oct 29, 2026)
- **Web3Forms access key: WIRED.** The sample form posts to `https://api.web3forms.com/submit` and `index.html` already carries the real access key for mitchell@msecomllc.com (placeholder replaced — verified by grep Oct 29, 2026). The form's hidden `subject` is exactly **"FreshAuthority Sample Request"** — do not change it; the fulfillment script matches on it.
- Everything else on the page is final: copy, volumes (verified against the FMCSA file dated 2026-10-29), and Stripe links (below) are live.
- **Fulfillment script fixed Oct 29, 2026:** `hidden_files/pipeline/fulfill_samples.py` originally failed to compile (the Stripe token swap had replaced the LINK_39/79/179 variable names with the URLs). Assignments restored; `python3 -m py_compile` passes. The old $19/$99/$249/$499 links below were DEACTIVATED — never reintroduce them.

## Stripe checkout links — LIVE (main MS ECOM account, verified by HQ Oct 29, 2026; pricing pivot — free 7-day trial on every plan, card required)
| Slot | Price | URL |
|---|---|---|
| One State | $39/mo | https://buy.stripe.com/aFaeVfeFoh0MgofeuhbjW0e |
| Every State | $79/mo | https://buy.stripe.com/aFadRbgNwaCob3Vbi5bjW0f |
| Every State Plus Feeds | $179/mo | https://buy.stripe.com/4gMdRbdBkcKwdc371PbjW0g |

Wired into `site/index.html` (verified by grep Oct 29, 2026: only these three links, $39/$79/$179 prices) and into `hidden_files/pipeline/fulfill_samples.py` as LINK_39 / LINK_79 / LINK_179 (sample-delivery email). The original ladder ($19 trial / $99 / $249 / $499) was deactivated Oct 29, 2026 after HaulerCheck's live pricing undercut it — see `hidden_files/pricing-decision-2026-10-29.md`.

## Numbers on the page (all verified from pipeline output, FMCSA file dated 2026-10-29, 30-day window, for-hire carrier scope)
6,193 grants nationwide · TX 717 · CA 624 · FL 499 · IL 389 · OH 325 · PA 316 · GA 251 · NC 212 · NJ 192 · IN 155 · AZ 133 · MI 128 · TN 127.
Refresh these when HQ re-runs the 30-day pipeline (numbers are labeled "as of" on the page — keep the label honest).

## Post-deploy checks
1. Submit a test sample request → confirm the email lands at mitchell@msecomllc.com with subject "FreshAuthority Sample Request" → run the fulfillment cron once by hand and confirm the sample CSV comes back attached.
2. Click each Stripe link once → confirm correct product/price on the checkout page.
3. Mobile pass: pricing grid collapses to one column; nav keeps the sample CTA.
