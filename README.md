# Load Lab — Cal vehicle size selector

Staff paste a customer's freight email, the tool parses it (Claude AI with a regex
fallback), runs a strict 3D bin-pack against the Cal North fleet, and recommends the
smallest vehicle plus body spec (curtain / tail lift), with a navigable 3D load plan.

**Live:** https://owenjohnson505.github.io/cal-loadlab/

## Architecture

| Piece | Where | Notes |
|---|---|---|
| Web app | This repo → GitHub Pages (`index.html`, single file) | All packing logic runs in the browser; no freight data is stored anywhere |
| Auth | Supabase **cal-toolkit** project (`ljofgxvmshetkhznqxaf`) | Email + password against the existing staff user pool (same logins as the toolkit). Add new staff in Supabase → Authentication → Users |
| AI parsing | Edge function `parse-freight` (same project) | Verifies the staff JWT, restricts to `@cal.delivery`, rate-limits 200 parses/user/day (`loadlab_ai_usage` table), then calls Claude (`claude-haiku-4-5`) with a strict JSON schema. Falls back to the built-in regex parser if unavailable |
| API key | Edge function secret `ANTHROPIC_API_KEY` | Never present in this repo or the page |

## Deploying changes

Edit `index.html`, commit, push — GitHub Pages redeploys automatically.
The canonical development copy of the tool lives in the Desktop `design` folder
(`Vehicle-Selector.html`); this file is that tool plus the auth gate + AI hook.

## Security notes

- The page is public but renders only a sign-in wall without a staff session.
- The AI endpoint rejects: no JWT (401), non-`@cal.delivery` accounts (403),
  over-limit users (429). The Anthropic key lives only in function secrets.
- The `loadlab_ai_usage` table has RLS enabled with no policies — only the
  service role (inside the function) can touch it.
