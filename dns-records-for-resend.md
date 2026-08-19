# DNS records to add for cal.delivery (email sending via Resend)

Please add the following **three** DNS records to the cal.delivery zone.
They only affect a new `send.` subdomain and a DKIM key — **they do not touch
the website, existing email (Office 365), or the main SPF record.**

| # | Type | Host / Name | Value | TTL | Priority |
|---|------|-------------|-------|-----|----------|
| 1 | TXT | `resend._domainkey` | `p=MIGfMA0GCSqGSIb3DQEBAQUAA4GNADCBiQKBgQDGLKEKI/pdNpnIBUp6/6ESVu1t9CRodWX9qDpP2O+ANThwjEHOZLMpn7G0wwxwXFA/gjjaEQqz68WTY39+8aAJ8xlt7/aO2IjlBFrwCcdzrTgzhlF37LA07EcXCq7dyxRGne0JF9lzLjc0zQccO1uADPjkdcxhhqO8sf5caGrJ3wIDAQAB` | Auto/3600 | — |
| 2 | MX | `send` | `feedback-smtp.eu-west-1.amazonses.com` | Auto/3600 | 10 |
| 3 | TXT | `send` | `v=spf1 include:amazonses.com ~all` | Auto/3600 | — |

Notes for whoever applies this:
- "Host/Name" is relative to cal.delivery (so record 1 is `resend._domainkey.cal.delivery`, records 2–3 are `send.cal.delivery`).
- Do **NOT** add or change any MX record on the root domain (@) — company email must stay on Office 365.
- No changes to the existing root TXT/SPF record.
