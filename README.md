# Disposable email domains

A curated list of the top 1,000 disposable email domains, updated daily.

Maintained by [Castle's research team](https://castle.io/research/).

## Why this list exists

There are already many public disposable email domain lists. Most are community-maintained, updated through pull requests, and gradually grow into large catch-all datasets mixing:

- Disposable email providers
- Privacy-focused email services
- Custom domains with unclear ownership

That creates two common problems:

1. False positives when used for blocking
2. Large noisy lists that are difficult to operationalize

We built this repository with a different philosophy.

- **Updated daily.** Fully automated pipeline, no dependency on community submissions.
- **Curated, not aggregated.** We do not import domains from public lists. Every domain is independently verified and tied to a real disposable email service.
- **Strictly disposable.** We intentionally exclude privacy-oriented providers such as Proton Mail or Tuta. Their primary purpose is privacy, not throwaway account creation.
- **Observed in real abuse.** These domains are actively used in fake signups, promo abuse, spam, multi-accounting, and other attacks observed across Castle's network.
- **Small and focused.** Only 1,000 domains. Smaller lists are easier to maintain, faster to query, and less likely to generate false positives.

If a domain is included here, there is a strong reason for it.

## How we collect domains

We combine passive and active collection techniques.

### Website scraping

We continuously scrape known disposable email provider websites to identify the domains they serve.

New providers appear regularly, often exposing large pools of rotating domains.

### DNS analysis

We analyze DNS records such as MX and A records to identify domains sharing infrastructure with known disposable email services.

This helps uncover:

- White-label disposable email services
- Hidden custom domains
- Disposable domains absent from public lists

### Real-world abuse telemetry

Castle protects large consumer platforms against fraud and bot attacks.

Domains are ranked based on observed abuse activity across Castle's network, including:

- Fake account creation
- Multi-accounting
- Promo abuse
- Spam campaigns

The result is a dataset that reflects current abuse patterns.

## Repository contents

### Main list

```text
disposable-email-domains.txt
```

Plain text format:

```text
example-disposable.com
tempmail.net
throwaway.email
```

- One domain per line
- Sorted by observed abuse volume
- Most frequently abused domains first

## Usage

Fetch the latest version directly from GitHub:

```bash
curl -sL https://raw.githubusercontent.com/castle/disposable-email-domains/main/disposable-email-domains.txt
```

The dataset is intentionally small enough to load entirely into memory.

### Example (Python)

```python
import urllib.request

URL = "https://raw.githubusercontent.com/castle/disposable-email-domains/main/disposable-email-domains.txt"

disposable_domains = set(
    urllib.request.urlopen(URL)
    .read()
    .decode()
    .splitlines()
)

def is_disposable(email: str) -> bool:
    domain = email.rsplit("@", 1)[-1].lower()
    return domain in disposable_domains
```

### Example (JavaScript / Node.js)

```javascript
const response = await fetch(
  "https://raw.githubusercontent.com/castle/disposable-email-domains/main/disposable-email-domains.txt"
);

const text = await response.text();

const disposableDomains = new Set(
  text.split("\n").map(d => d.trim()).filter(Boolean)
);

function isDisposable(email) {
  const domain = email.split("@").pop().toLowerCase();
  return disposableDomains.has(domain);
}
```

## What this list is not

### Not exhaustive

This repository only contains the top 1,000 disposable email domains currently observed in abuse activity.

Castle's internal dataset is significantly larger and continuously evolving.

If you need broader coverage, see the [Castle API](https://docs.castle.io/).

### Not a blocking recommendation

How you use this data is up to you.

Different teams use disposable email signals differently:

- Hard blocking
- Risk scoring
- Step-up verification
- Rate limiting
- Manual review

Disposable email usage alone is not always malicious.

## Updates

The list is regenerated automatically every day and committed through an automated pipeline.

## License

MIT

## Links

- [Castle](https://castle.io/) - Account security and fraud prevention
- [Castle Research](https://castle.io/research/) - Threat research and technical articles
- [Castle Docs](https://docs.castle.io/) - API and product documentation