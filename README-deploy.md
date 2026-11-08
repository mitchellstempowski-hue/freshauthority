# FreshAuthority — storefront deploy notes

## Contents
- `index.html` — the entire storefront (single file, inline CSS, no external assets, no JS). Premium single-page: hero with real dated volumes → mechanism → file contents (16 fields) → three-files-in-every-tier → state volume table → how it works → pricing → factoring section → free-sample form → FAQ → legal footer.

## Deploy
- Publish as a **subdomain on existing hosting** (e.g. `freshauthority.<existing-domain>` or its own domain when HQ assigns one). Pure static — drop `index.html` at the docroot. No build step, no server-side code.
- HQ handles the deploy itself (per the build brief).

## Before go-live — wires status (updated Oct 29, 2026)
- **Web3Forms access key: WIRED.** The sample form posts to `https://api.web3forms.com/submit` and `index.html` already carries the real access key for mitchell@msecomllc.com (placeholder replaced — verified by grep Oct 29, 2026). The form's hidden `subject` is exactly **"FreshAuthority Sample Request"** — do not change it; the fulfillment script matches on it.
- Everything else on the page is final: copy, volumes (verified against the FMCSA file dated 2026-10-29), and Stripe links (below) are live.
- **Fulfillment script fixed Oct 29, 2026:** `hidden_files/pipeline/fulfill_samples.py` originally failed to compile (the Stripe token swap had replaced the LINK_39/79/179 variable names with the URLs). Assignments restored; `python3 -m py_compile` passes. All superseded link sets are DEACTIVATED — never reintroduce them.

## Stripe checkout links — LIVE (main MS ECOM account; REPRICE Nov 8, 2026 — free 7-day trial on every plan, NO CARD to start)
| Slot | Price | URL |
|---|---|---|
| All States | $49/mo | https://buy.stripe.com/7sY4gB40K39W8VN0DrbjW0z |
| All States Annual | $490/yr | https://buy.stripe.com/9B6bJ37cW11Ob3Vcm9bjW0A |
| Plus Feeds | $99/mo | https://buy.stripe.com/6oUfZjcxg5i40ph85TbjW0B |
| Plus Feeds Annual | $990/yr | https://buy.stripe.com/5kQ14pbtch0Meg70DrbjW0C |

Wired into `site/index.html` and the generated pages (via `hidden_files/pipeline/build_site_data.py` LINK_49 / LINK_99) and into the pipeline email scripts as LINK_49 / LINK_99. The previous ladder (One State $39 / Every State $79 / Plus Feeds $179 + annuals) was deactivated Nov 8, 2026 — see `hidden_files/reprice-log-2026-11-08.md`. The original launch ladder ($19 trial / $99 / $249 / $499) was deactivated Oct 29, 2026 — see `hidden_files/pricing-decision-2026-10-29.md`.

## Numbers on the page (all verified from pipeline output, FMCSA file dated 2026-10-29, 30-day window, for-hire carrier scope)
6,193 grants nationwide · TX 717 · CA 624 · FL 499 · IL 389 · OH 325 · PA 316 · GA 251 · NC 212 · NJ 192 · IN 155 · AZ 133 · MI 128 · TN 127.
Refresh these when HQ re-runs the 30-day pipeline (numbers are labeled "as of" on the page — keep the label honest).

## Post-deploy checks
1. Submit a test sample request → confirm the email lands at mitchell@msecomllc.com with subject "FreshAuthority Sample Request" → run the fulfillment cron once by hand and confirm the sample CSV comes back attached.
2. Click each Stripe link once → confirm correct product/price on the checkout page.
3. Mobile pass: pricing grid collapses to one column; nav keeps the sample CTA.
