# A2P 10DLC Campaign — Resubmission Field Text

Paste-ready answers for the Twilio Console "Revise errors and resubmit your A2P Campaign
registration" form. Written 2026-09-05 in response to the first submission's two rejection reasons:

1. **"…rejected due to issues verifying the Call to Action (CTA) provided for the campaign."**
2. **"…rejected because of invalid campaign description."**

## What changed to fix them

- **CTA:** `consent-form.md` was rewritten as a single, unambiguous, consumer-initiated CTA (text
  `START` to the number) with the full consent disclosure block directly adjacent to it. The mock
  consent checkbox (an unverifiable second opt-in method) was removed, dead cross-links were fixed
  (`./privacy-policy` → `./privacy-policy.md`, which resolves on GitHub), and a real contact email
  was added to every page. **The pages must be publicly reachable before resubmitting** — the first
  rejection almost certainly happened because the reviewer had no live URL to verify.
- **Description:** replaced with the explicit who/what/whom/how-consented/frequency description
  below — vague "personal use" descriptions get rejected.

## Before you resubmit — checklist

- [ ] Push the four `docs/twilio/*.md` pages to the **public** GitHub repo.
- [ ] Open the consent-form URL in a private/incognito window — it must load with **no login** and
      the Privacy Policy / Terms links must work.
- [ ] Fill the real URLs into the fields below (replace `{{…}}` placeholders).
- [ ] Confirm the Twilio number in the CTA (+1 651 661 0582) matches the campaign's number.

Record the final URLs here once pushed:

- CTA / consent form: `{{CTA_URL}}`
- Privacy Policy: `{{PRIVACY_URL}}`
- Terms of Service: `{{TOS_URL}}`

> Tip: use the **rendered** GitHub page URL (`github.com/<user>/<repo>/blob/main/...md`), not the
> `raw.githubusercontent.com` URL — reviewers need to see a readable page.

---

## Campaign description

> Frank House Dashboard is a private household notification service operated by a sole proprietor
> for personal, non-commercial use. Household members (the account owner and immediate family —
> fewer than 10 people total) send a short text note to this number to have it shown on an in-home
> display; the service replies to each note with a single transactional SMS confirming the note was
> received and displayed. Messages are sent only to individuals who first text the number themselves
> (consumer-initiated opt-in via the keyword START). No marketing, promotional, or third-party
> messages are ever sent. Message frequency varies, typically a few messages per month per user.
> Recipients can reply STOP to opt out or HELP for help at any time.

## Message flow / how end users consent (CTA field)

> End users opt in by sending a message first: a household member texts the keyword START from their
> own mobile phone to this number. That consumer-initiated message is the opt-in event; the service
> never sends a message to a number that has not first texted it, and no numbers are imported,
> purchased, or enrolled by anyone else. The publicly viewable call-to-action and consent
> disclosures are posted at {{CTA_URL}}. The disclosure includes message frequency, "Message and
> data rates may apply," STOP/HELP instructions, and links to the Privacy Policy ({{PRIVACY_URL}})
> and Terms of Service ({{TOS_URL}}). Replying STOP opts the user out immediately; HELP returns
> help information.

## Sample messages

Sample 1 (note confirmation — matches the reply sent by
`docs/ha/dashboard_note_package.yaml`):

> FrankHouseDashboard: Note received and displayed: "Pick up milk on the way home". Reply STOP to
> opt out, HELP for help.

Sample 2 (opt-in confirmation):

> FrankHouseDashboard: You are opted in to dashboard note confirmations. Message frequency varies.
> Msg & data rates may apply. Reply STOP to opt out, HELP for help.

## Other campaign attributes

| Field | Answer |
|---|---|
| Use case | Sole Proprietor |
| Subscriber opt-in | Yes — keyword `START` (consumer-initiated) |
| Subscriber opt-out | Yes — `STOP` (Twilio Advanced Opt-Out handles the reply) |
| Subscriber help | Yes — `HELP` |
| Embedded links in messages | No |
| Embedded phone numbers in messages | No |
| Age-gated content | No |
| Direct lending / loan arrangement | No |

Suggested HELP response (if the console asks for one):

> FrankHouseDashboard: A private household note service. A confirmation SMS is sent when you text a
> note. Contact frankha813@gmail.com. Reply STOP to opt out.

Suggested opt-out (STOP) response:

> FrankHouseDashboard: You are opted out and will receive no further messages. Text START to
> re-subscribe.

**Console check:** in Messaging → Services (or the number's Opt-Out Management), confirm
**Advanced Opt-Out** is enabled so STOP/HELP/START get compliant automatic replies at the Twilio
layer — no Home Assistant automation is needed for them.

---

## After approval

1. Uncomment the confirmation-reply action in `docs/ha/dashboard_note_package.yaml` (section 3) and
   paste the real Account SID into the `rest_command` URL (section 4).
2. Reload the package / restart HA; text a note; confirm the SMS reply arrives and the note renders.
3. At go-live, repoint the Twilio inbound webhook to the deployment HA's cloud webhook URL (see the
   WEBHOOK block at the bottom of the package yaml).
