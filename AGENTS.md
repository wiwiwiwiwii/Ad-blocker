# AGENTS.md

## Purpose

This repository maintains a small, user-specific Stash ad-blocking supplement for iOS/iPadOS.

The deployed stack is intentionally modular:

1. **Shiina AdBlock Lite** is the primary maintained base.
2. **Shiina Startup Ads** is the generic startup/splash-ad supplement.
3. **`Extra-AdBlock.stoverride`** contains only additional rules for apps the user actually has installed and that are not already adequately covered by the two Shiina modules.

The goal is not maximum rule count. The goal is high practical coverage with a small MITM surface, low false-positive risk, and no obvious breakage of core app functionality.

---

## Repository Scope

Expected repository:

- Repository: `wiwiwiwiwii/Ad-blocker`
- Default branch: `main`
- Main custom override: `Extra-AdBlock.stoverride`
- Maintenance instructions: `AGENTS.md`
- User-facing overview: `README.md`

Do not modify unrelated repositories, especially `dwss-official-account`.

---

## Currently Deployed External Overrides

These are installed separately in Stash and **must not be copied wholesale into `Extra-AdBlock.stoverride`**.

### 1. Shiina AdBlock Lite

Repository:
`https://github.com/ShiinaWong/stash-configs`

Raw:
`https://raw.githubusercontent.com/ShiinaWong/stash-configs/main/overrides/shiina-adblock-lite.stoverride`

Stash install:
`https://link.stash.ws/install-override/raw.githubusercontent.com/ShiinaWong/stash-configs/main/overrides/shiina-adblock-lite.stoverride`

Current known scope includes:

- Bilibili
- Cainiao
- Tieba
- Zhihu
- Taobao
- JD
- Pinduoduo
- Xianyu
- Xiaohongshu
- SMZDM
- Ctrip

It also contains a small number of generic ad-domain rules.

Do not duplicate these app modules in Extra unless a newer upstream source provides a **specific missing low-risk rule** that is not already handled by Shiina Lite.

### 2. Shiina Startup Ads

Raw:
`https://raw.githubusercontent.com/ShiinaWong/stash-configs/main/overrides/modules/startup-ads.stoverride`

Stash install:
`https://link.stash.ws/install-override/raw.githubusercontent.com/ShiinaWong/stash-configs/main/overrides/modules/startup-ads.stoverride`

This is installed separately.

Important overlap note:

- `startup.umetrip.com`
- `discardrp.umetrip.com`

are already handled here.

Therefore `Extra-AdBlock.stoverride` should not duplicate the generic Umetrip startup/discard rewrite. Extra may still contain the more precise R-Store Umetrip `home / umerp / bkclient` native-response script because that behavior is not supplied by Startup Ads.

### 3. Custom Extra AdBlock

Target raw URL:
`https://raw.githubusercontent.com/wiwiwiwiwii/Ad-blocker/main/Extra-AdBlock.stoverride`

Stash install:
`https://link.stash.ws/install-override/raw.githubusercontent.com/wiwiwiwiwii/Ad-blocker/main/Extra-AdBlock.stoverride`

Before deployment, verify the actual GitHub owner/repository spelling and confirm the Raw URL returns the expected file.

Do not silently "fix" repository URLs without verifying the actual repository path first.

---

## Overrides That Should Remain Disabled / Not Installed

To avoid duplicate processing and unnecessary MITM:

- Blackmatrix7 `AdvertisingLite`
- Blackmatrix7 `AllInOne`
- Blackmatrix7 legacy `开屏去广告`
- Standalone Shiina Taobao module when Shiina Lite is enabled
- Standalone Shiina JD module when Shiina Lite is enabled
- Separate CMCC module when equivalent CMCC rules are already present in Extra
- Shiina Core DNS / 5k+ generic domain list, unless explicitly requested later

The current design is:

`Shiina AdBlock Lite + Shiina Startup Ads + Extra AdBlock`

Do not add a fourth large generic ad-block base without explicit approval.

---

## Primary Reference Sources

Use maintained upstreams as sources, not as blindly imported bundles.

### Priority 1: Stash-native, actively maintained

#### ShiinaWong/stash-configs

`https://github.com/ShiinaWong/stash-configs`

Use as the primary base and first place to check for modern Stash-native rules.

#### qsoyq/stash

`https://github.com/qsoyq/stash`

Useful for narrow Stash-native app-specific overrides, especially carrier apps such as China Mobile.

### Priority 2: Current app-specific rule projects

#### Kelee / Romeo mirror

`https://github.com/ifflagged/Romeo`

Relevant paths:

- `Modules/Loon/Kelee/`
- `Modules/JavaScript/Kelee/`

Use Kelee mainly to identify modern app-specific endpoints and low-risk cleanup opportunities.

Do **not** automatically copy all Kelee behavior. Many Kelee modules intentionally alter app UI, recommendations, tabs, membership presentation, download restrictions, or other non-ad behavior.

#### R-Store

`https://github.com/zirawell/R-Store`

Relevant paths:

- `Rule/Surge/Adblock/App/`
- `Res/Scripts/AntiAd/`

Useful for current app-specific endpoints and scripts.

Convert only the necessary behavior to Stash syntax.

#### fmz200/wool_scripts

`https://github.com/fmz200/wool_scripts`

Useful for current splash endpoints and app-specific rules. Romeo may contain mirrors of these modules.

### Priority 3: Legacy/reference-only

#### blackmatrix7/ios_rule_script

`https://github.com/blackmatrix7/ios_rule_script`

Use only for historical discovery or cross-checking.

Do not use Blackmatrix7 Stash aggregate files as the current base because their generated aggregate content is significantly older than the current Shiina/Kelee/R-Store sources.

---

## Rule Admission Policy

A new rule may be added to `Extra-AdBlock.stoverride` when all of the following are true:

1. The app is actually installed or is a high-priority app for the user.
2. The ad/marketing endpoint is supported by a maintained source or confirmed from live Stash traffic.
3. The rule is not already adequately handled by Shiina Lite or Shiina Startup Ads.
4. The endpoint meaning is sufficiently narrow and understandable.
5. The expected benefit is meaningful: splash ad, feed ad, popup, marketing banner, ad card, search marketing, promotional recommendation, etc.
6. The rule does not have an obvious risk of breaking login, payment, purchase, booking, navigation, messaging, order management, account security, or other core functions.

When uncertain, leave the rule out and report it for review.

---

## Allowed Low-Risk Cleanup Beyond Splash Ads

The user explicitly allows Kelee-style cleanup beyond startup ads when side effects are not obvious.

Generally acceptable:

- Splash ads
- Feed ads
- Promotional banners
- Marketing popups
- Floating promo widgets
- Ad cards
- Search hot words / marketing search suggestions
- Promotional recommendation modules
- Red-dot marketing prompts
- Coupon marketing popups
- Activity/commerce promotions that do not affect core app functions
- Pure ad SDK endpoints
- Dedicated ad domains

Review carefully before adding:

- General recommendation feeds
- Homepage modules
- "My" page modules
- Navigation tabs
- Membership cards
- Activity centers
- Wallet-related content
- Order-related content

---

## Explicitly Forbidden / High-Risk Behavior

Do not add these without explicit user approval:

- VIP or subscription spoofing
- Fake membership levels or expiration dates
- Unlocking paid features
- Modifying purchase/entitlement state
- Modifying account identity or follow relationships
- Removing security checks
- Disabling certificate pinning or anti-fraud controls
- Login bypasses
- Payment or checkout manipulation
- Order-state manipulation
- Broad deletion of core navigation tabs
- Broad replacement of an app homepage with a reduced whitelist
- Removing core travel, booking, navigation, messaging, or account-management functions
- Rules whose main purpose is watermark removal or download restriction bypass rather than ad cleanup

Examples already rejected during review:

- Kelee NetEase Cloud Music VIP/account-state modifications
- Aggressive Didi homepage/navigation whitelisting
- Aggressive Amap "My" page / POI page structural deletion
- Broad China Mobile `aggregationData` blocking that also removes useful login/tool UI
- Kelee Xiaohongshu watermark/download modifications
- Large Pinduoduo UI/order structure rewrites
- JD personal-center rewrites that remove large amounts of normal functionality

---

## Sensitive App Policy

For banking, payment, brokerage, government, identity, and medical apps, default to **no MITM**.

Examples:

- ICBC
- Bank of China
- CMB
- HSBC
- UnionPay
- PayPal
- IBKR
- OKX
- credit-card banking apps
- tax apps
- national medical insurance apps
- government-service apps
- immigration/identity apps

Even if public ad-cleaning rules exist, do not add them merely for cosmetic cleanup.

A narrow non-MITM domain reject may still be considered if it is clearly an independent ad host, but it must be reviewed explicitly.

---

## Current Extra Coverage

`Extra-AdBlock.stoverride` currently targets or is expected to target the following user-installed/high-priority apps.

### Finance / information

- Jin10
- Eastmoney
- Tonghuashun
- Xueqiu
- CLS
- WallstreetCN
- Jiemian
- Tiantian Fund

### Travel / transport

- Umetrip
- Railway 12306
- Traffic Management 12123
- Meituan
- Dianping
- Amap
- Didi
- China Southern
- China Eastern
- Huazhu
- Hilton China
- Atour

### Carriers

- China Mobile
- China Unicom

### Social / comics

- Weibo
- Bilibili Comics

### Music / reading / utilities

- Ximalaya
- NetEase Cloud Music
- QQ Music
- iReader
- Mail Master
- Baidu Netdisk
- Fitdays
- Mi Home

### Jobs / shopping

- Liepin
- Sam's Club

Do not assume this list itself proves rules are correct. It is only the intended coverage set.

---

## Apps Already Covered by Shiina Lite

Do not recreate their basic ad-removal logic in Extra.

Current important examples:

- Taobao
- JD
- Pinduoduo
- Xianyu
- Xiaohongshu
- Cainiao
- Zhihu
- SMZDM
- Ctrip
- Bilibili
- Tieba

Extra may supplement them only when a maintained Kelee/R-Store source contains a **specific low-risk behavior that Shiina Lite currently lacks**.

Examples of acceptable supplements:

- JD search/recommendation marketing endpoints
- Pinduoduo narrow search/recommendation endpoints
- Xianyu search/promotional recommendation endpoints
- Xiaohongshu search marketing/hot-word endpoints

Avoid adding full Kelee modules for these apps because that would duplicate Shiina and often introduce aggressive UI modifications.

---

## Pre-Deployment Validation

Before committing any release, run all checks below.

### A. Repository safety

- Confirm repository is exactly `wiwiwiwiwii/Ad-blocker`.
- Confirm branch is `main`.
- Confirm no unrelated files are modified.
- Confirm `dwss-official-account` is untouched.

### B. YAML validation

Parse `Extra-AdBlock.stoverride` as YAML.

Fail deployment if:

- YAML cannot be parsed.
- Tabs are used for indentation.
- Duplicate YAML keys occur.
- `http`, `rules`, or `script-providers` are malformed.
- A script provider name does not match its `http.script` reference.

### C. Stash structure validation

Check:

- `rules` entries use valid Stash rule forms.
- `http.mitm` is a list of valid host strings.
- `http.url-rewrite` entries contain a regex followed by a supported action.
- Expected actions are limited to known Stash forms such as:
  - `reject`
  - `reject-200`
  - `reject-dict`
  - `reject-img`
- `http.script` entries have:
  - `match`
  - `name`
  - `type`
  - appropriate `require-body`
- every script provider URL is reachable.

### D. Regex validation

For every rewrite/match regex:

- check that it compiles where practical;
- check for accidental double escaping;
- check for missing anchors where a broad pattern could overmatch;
- check for malformed alternations;
- check for accidental matching of core API paths.

Do not rewrite a regex merely for style if the upstream form is known-good.

### E. MITM coverage validation

For HTTPS rewrite/script endpoints:

- confirm their host is included in `http.mitm`;
- avoid MITM hosts that are not needed by any rule;
- remove stale MITM hosts when the corresponding rule is removed;
- prefer exact hosts over broad wildcards.

Report the number of MITM hosts before and after every release.

A large unexplained increase is a deployment blocker.

### F. Cross-module overlap validation

Fetch the current raw versions of:

1. Shiina AdBlock Lite
2. Shiina Startup Ads
3. Extra AdBlock

Check for:

- identical URL rewrite patterns;
- identical script matches;
- duplicate app-specific splash handling;
- duplicate Umetrip startup/discard handling;
- standalone Taobao/JD behavior already supplied by Shiina Lite.

Exact duplicates should normally be removed from Extra.

An overlapping hostname is acceptable only when Extra adds genuinely different behavior on the same host.

### G. Upstream script review

For every remote script provider referenced by Extra:

- fetch the current script;
- confirm it still performs only the expected ad-cleanup behavior;
- inspect recent upstream changes when possible;
- stop deployment if the script has expanded into account, entitlement, security, or unrelated UI modification.

Do not blindly trust a `main` branch script because its URL still works.

### H. Source freshness

For each newly added rule:

- record the source repository/module;
- prefer rules updated or still present in current 2026 upstreams;
- if only an old source exists, cross-check with at least one other source or live traffic;
- do not resurrect a stale endpoint just because Blackmatrix7 once contained it.

### I. Duplicate and dead-rule checks

Within Extra:

- remove exact duplicate rules;
- remove duplicate MITM hosts;
- remove duplicate domain rules;
- flag rules that are fully shadowed by a broader preceding rule;
- flag hosts with no corresponding rewrite/script/domain purpose.

### J. Sensitive-function review

Search changed rules for terms suggesting risk, including:

- login
- auth
- token
- pay
- payment
- order
- checkout
- security
- verify
- certificate
- pin
- wallet
- member
- vip
- entitlement
- subscription

These are not automatically forbidden, but any hit requires manual review before deployment.

### K. Versioning

For every deployed change:

- bump `version`;
- update `date`;
- keep `name` stable;
- summarize meaningful changes in the commit message.

Do not change behavior without changing the version.

---

## Static Validation Commands

A local agent may implement an automated validation script.

Recommended checks:

```bash
python - <<'PY'
from pathlib import Path
import yaml

p = Path("Extra-AdBlock.stoverride")
text = p.read_text(encoding="utf-8")

assert "\t" not in text, "Tabs found in YAML"

data = yaml.safe_load(text)
assert isinstance(data, dict)
assert data.get("name")
assert data.get("version")
assert isinstance(data.get("rules", []), list)
assert isinstance(data.get("http", {}), dict)

http = data.get("http", {})
assert isinstance(http.get("mitm", []), list)
assert isinstance(http.get("url-rewrite", []), list)
assert isinstance(http.get("script", []), list)

providers = data.get("script-providers", {})
for item in http.get("script", []):
    assert item["name"] in providers, f"Missing provider: {item['name']}"

mitm = http.get("mitm", [])
assert len(mitm) == len(set(mitm)), "Duplicate MITM hosts found"

rules = data.get("rules", [])
assert len(rules) == len(set(rules)), "Duplicate rules found"

rewrites = http.get("url-rewrite", [])
assert len(rewrites) == len(set(rewrites)), "Duplicate rewrites found"

print("Basic YAML/Stash structure OK")
print("MITM hosts:", len(mitm))
print("Domain rules:", len(rules))
print("URL rewrites:", len(rewrites))
print("Scripts:", len(http.get("script", [])))
PY
```

If PyYAML is unavailable, install it only in the local development environment; do not add a runtime dependency to the repository.

---

## Pre-Deployment Network Checks

Verify these return HTTP 200 before deployment:

```text
https://raw.githubusercontent.com/ShiinaWong/stash-configs/main/overrides/shiina-adblock-lite.stoverride
https://raw.githubusercontent.com/ShiinaWong/stash-configs/main/overrides/modules/startup-ads.stoverride
https://raw.githubusercontent.com/wiwiwiwiwii/Ad-blocker/main/Extra-AdBlock.stoverride
```

Also verify every `script-providers.*.url` in Extra.

---

## Release Workflow

1. Read `AGENTS.md`.
2. Fetch the two currently deployed Shiina overrides.
3. Fetch/review all upstream sources relevant to changed apps.
4. Validate the proposed `Extra-AdBlock.stoverride`.
5. Compare against Shiina Lite and Startup Ads for overlap.
6. Review every remote script provider.
7. Produce a concise validation report.
8. If any high-risk or ambiguous issue remains, stop and ask for review.
9. Otherwise commit only:
   - `Extra-AdBlock.stoverride`
   - `AGENTS.md` if this file itself changed
   - `README.md` if app coverage or install instructions changed
10. Commit to `main`.
11. Re-fetch the Raw file from GitHub.
12. Confirm version/content match the committed source.
13. Output the Raw URL and Stash install URL.

---

## Stash Installation

Expected user-facing stack:

Base

`https://link.stash.ws/install-override/raw.githubusercontent.com/ShiinaWong/stash-configs/main/overrides/shiina-adblock-lite.stoverride`

Startup supplement

`https://link.stash.ws/install-override/raw.githubusercontent.com/ShiinaWong/stash-configs/main/overrides/modules/startup-ads.stoverride`

User-specific Extra

`https://link.stash.ws/install-override/raw.githubusercontent.com/wiwiwiwiwii/Ad-blocker/main/Extra-AdBlock.stoverride`

After updating an already-installed remote override, refresh/update it in Stash rather than installing a duplicate copy.

No CA certificate reinstall is required merely because the override content changes.

---

## Smoke-Test Checklist After Deployment

Static validation is not enough.

After a material release, manually test a representative sample:

- Jin10
- Umetrip
- China Mobile
- China Unicom
- Amap
- Didi
- Railway 12306
- NetEase Cloud Music
- Ximalaya
- one airline app
- one hotel app

Also test core functionality:

- login still works;
- account pages load;
- travel search/booking works;
- maps/navigation still work;
- carrier account/balance pages still work;
- music playback works;
- order pages still work.

For Shiina-covered apps, spot-check:

- Taobao
- JD
- Xiaohongshu
- Xianyu
- Pinduoduo

If an app breaks, first disable Extra AdBlock only to isolate whether the problem is in the custom layer.

---

## Maintenance Principle

Prefer:

`small precise rule + known purpose + current source`

over:

`large aggregate + broad MITM + unknown side effects`

The repository should remain understandable enough that every rule can be explained by app, endpoint, expected effect, and upstream source.
