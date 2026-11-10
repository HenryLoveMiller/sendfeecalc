# sendfeecalc

**Published remittance rates and fees for Canada → India.** Two calculators, one source of truth: how much CAD to send, and how much your family actually receives.

Live site: **https://henrylovemiller.github.io/sendfeecalc/**

## What this is

Most money-transfer comparisons start from the amount you send. A lot of real transfers start from the other end: a fixed amount that has to land in India — a month's rent, a wedding, a medical bill. This site covers both directions.

| Page | What it answers |
|---|---|
| [CAD → INR calculator](https://henrylovemiller.github.io/sendfeecalc/) | You send X CAD — what does the recipient actually get, after the transfer fee and the exchange-rate spread? |
| [INR → CAD calculator](https://henrylovemiller.github.io/sendfeecalc/inr-to-cad-calculator.html) | They need X INR — how many CAD does each channel need to send so that amount lands? |
| [Remitly: welcome vs everyday rate](https://henrylovemiller.github.io/sendfeecalc/remitly-cad-to-inr.html) | Which of Remitly's two published rates applies to your transfer? |
| [Remitly vs Wise](https://henrylovemiller.github.io/sendfeecalc/remitly-vs-wise-canada-to-india.html) | The same figures against Wise, including what changes on your second transfer. |
| [ICICI Bank Canada → India](https://henrylovemiller.github.io/sendfeecalc/icici-bank-canada-money2india-rate.html) | What the Money2India offer delivers, and where it stops. |
| [Taptap Send fee](https://henrylovemiller.github.io/sendfeecalc/taptap-send-canada-to-india-fee.html) | What the published C$1.99 fee actually costs you. |
| [How long does it take](https://henrylovemiller.github.io/sendfeecalc/transfer-time-canada-to-india.html) | Each provider's own published delivery times, and why the Indian payout rail decides the wait more than the brand does. |
| [How much can I send](https://henrylovemiller.github.io/sendfeecalc/transfer-limits-canada-to-india.html) | Each provider's own published sending ceiling — and why the limit is set by your funding account, not by the brand. |
| [Sources and read dates](https://henrylovemiller.github.io/sendfeecalc/sources.html) | Which page each published figure came from, and when it was read. |

## How the numbers are produced

- **Published figures only.** Every rate and fee is copied from that provider's own published page, read on a stated date. Nothing is estimated, averaged, or interpolated.
- **One data source.** The rate table lives in `index.html`; the other pages read the same figures on the same dates. A check script compares them, so the pages cannot drift apart.
- **Caps stop the pricing.** Where an offer is capped (a promotional rate that applies only to the first C$1,000 or C$2,000), the row says "not priced" above that instead of extrapolating a rate the provider has not published.
- **Mid-market is the benchmark, and it is dated.** The reference is the European Central Bank rate for the same business date as the provider rates. Rows published on a different day are measured against that day's reference, never against today's.
- **Not paid by anyone.** No provider pays this site. If referral links are ever added, they will be labelled at the point they appear, on the page where they appear.

## Read dates

The provider figures currently shown were read on **3 November 2026**, except where a row states its own earlier date (the ICICI NRI-desk rate carries a 30 October stamp on the provider's own table; the Taptap rate is a tracker's reading dated 4 November, and its fee was read 6 November). The `sources` page records the exact page and date for every figure.

## Development

The site is static HTML on GitHub Pages. There is no build step and no framework.

```bash
# 预渲染 + 推送 + 验收（推送前有闸门：sitemap 非法或含死链 = 不推）
./scripts/deploy.sh

# 单文件推送（上线新页面的正确顺序：先推页，页面 200 之后再把 sitemap 加上它）
./scripts/push_one.sh some-page.html

# 独立复核反向计算器页的每一个数字（不复用页面自己的公式）
node scripts/verify-inr-to-cad.js

# 提交 IndexNow（Bing / Yandex / Naver 收录通道）
python3 scripts/indexnow_push.py https://henrylovemiller.github.io/sendfeecalc/inr-to-cad-calculator.html
```

`scripts/prerender.js` and `scripts/prerender-inr.js` write the JavaScript-rendered rate tables back into the static HTML, so a crawler that never runs JavaScript still reads the numbers. `scripts/deploy.sh` re-renders and re-verifies before every push, and fails the deploy if a check fails.

## Contact

See the [privacy and contact page](https://henrylovemiller.github.io/sendfeecalc/privacy.html).
