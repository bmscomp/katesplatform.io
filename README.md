# Kates Platform Website (`katesplatform.io`)

This directory contains the static website and interactive 5-template studio for **Kates Platform** (`katesplatform.io` and `https://bmscomp.github.io/katesplatform.io/`).

## Included 5 Templates

You can switch between all 5 templates live using the top **Template Studio** bar, keyboard keys `1`–`5`, or URL query parameters:

| # | Template Name | Aesthetic & Best Use Case | URL Parameter |
|:-:|---------------|---------------------------|---------------|
| **1** | **Telemetry Mission Control** | Dark obsidian (`#090D16`) with amber/cyan telemetry, interactive HdrHistogram percentile curve simulator, and 3-zone KRaft health indicators. Best all-around technical product homepage. | `?template=1` |
| **2** | **Swiss Editorial & Book Press** | Warm ivory paper (`#FAF7F2`) with `EB Garamond` serif typography, 3D book cover showcase, and direct navigation into all 6 Parts of *Kates: The Definitive Guide*. | `?template=2` |
| **3** | **Terminal-First Developer Hub** | Monospace CLI/TUI showcase (`#0A0E14`) with interactive terminal tabs (`kates deploy`, `kates test run`, `kates cluster conformity`, `kates migrate`) and the `kates lab` Bubble Tea TUI. | `?template=3` |
| **4** | **Cloud-Native Bento Grid** | Linear/Vercel-inspired dark glassmorphic Bento Grid highlighting the 7 OCI-signed Helm charts, HdrHistogram SLA gates, and multi-arch containers. | `?template=4` |
| **5** | **Chaos & Resilience War Room** | Tactical SRE GameDay blueprint (`#071022`) with an interactive fault-injection simulator (`BROKER_KILL`, `CONTROLLER_KILL`, `NETWORK_PARTITION`). | `?template=5` |

Add `&clean=1` to any URL (or pass `--lock-template` at deploy time) to hide the top template-switcher bar.

---

## Deploying to `https://bmscomp.github.io/katesplatform.io/`

Run the deployment script from the repository root:

```bash
# Deploy with Template 1 as default (with the 5-template switcher bar enabled)
./scripts/deploy-website.sh

# Or lock a specific template (e.g. Template 4) without the switcher bar:
./scripts/deploy-website.sh --template 4 --lock-template
```

The script automatically:
1. Creates the public `bmscomp/katesplatform.io` GitHub repository if it does not exist yet.
2. Pushes the static bundle (`.nojekyll`, `index.html`, `assets/`) to the `main` branch of `bmscomp/katesplatform.io`.
3. Enables GitHub Pages on `bmscomp/katesplatform.io` so `https://bmscomp.github.io/katesplatform.io/` serves immediately.

---

## Binding Your Custom Domain (`katesplatform.io`)

Note: GitHub Pages redirects `https://bmscomp.github.io/katesplatform.io/` to `https://katesplatform.io/` as soon as a `CNAME` file is present. Keep `CNAME` disabled while testing on `bmscomp.github.io/katesplatform.io`, and enable it once your DNS records are live.

### Step 1 — Configure DNS Records at Your Domain Registrar

Add the four GitHub Pages `A` records for the apex domain (`katesplatform.io`) and a `CNAME` record for `www`:

| Type | Host / Name | Value / Target | TTL |
|------|-------------|----------------|-----|
| `A` | `@` | `185.199.108.153` | 3600 |
| `A` | `@` | `185.199.109.153` | 3600 |
| `A` | `@` | `185.199.110.153` | 3600 |
| `A` | `@` | `185.199.111.153` | 3600 |
| `CNAME` | `www` | `bmscomp.github.io.` | 3600 |

### Step 2 — Activate Custom Domain on GitHub Pages

Once DNS resolves (`dig katesplatform.io +short`), run:

```bash
./scripts/deploy-website.sh --enable-custom-domain
```

This writes `CNAME` (`katesplatform.io`) and configures GitHub Pages to provision the Let's Encrypt TLS certificate for `https://katesplatform.io`.
