# WeChat Mini Program Submission Checklist

Use this before upload or review submission.

## Project Basics

- App name, navigation title, project name, README, and visible UI names are consistent.
- `project.config.json` uses the intended official AppID.
- `app.json` includes only routes intended for review.
- Every route has `.js`, `.json`, `.wxml`, `.wxss`.
- No demo-only labels such as "Demo", "prototype", or "待接入" remain in user-facing flows unless intentionally shown as version limitations.

## Personal-Subject Risk Cleanup

For personal-subject mini programs, remove or hide:

- member invitation and user collaboration
- shared account books
- AA splitting, settlement, repayment, transfer, or collection flows
- payment collection, lending, investment, wealth management, rebate, affiliate, coupon arbitrage
- mandatory official-account follow prompts
- jumps to unrelated third-party mini programs
- reward-ad gating for essential utility functions

Prefer deleting unused pages from `app.json` and deleting orphan files if they still contain risky text.

## Privacy and Legal

Include in-app pages or accessible text for:

- privacy policy
- user agreement
- version notes / changelog
- feedback/contact path

For local-only tools, say:

- data is entered by the user
- data is stored in WeChat Mini Program local cache
- data is not uploaded to a server
- exported content is handled by the user

If cloud sync is added later, update the policy and privacy guide before release.

## Functional Smoke Test

On WeChat Developer Tools and ideally real-device preview, test:

- launch home page
- create a record
- edit a record
- delete a record
- filter/list records
- manage categories
- manage accounts
- import sample CSV or supported format
- export data and paste result elsewhere
- open user agreement/privacy/version pages

## Suggested Review Note

本小程序为个人本地工具。用户主动录入数据，数据默认保存在微信小程序本地缓存中。首版不提供支付、借贷、理财、返利、多人协作、资金分摊、公众号关注或无关第三方跳转功能。
