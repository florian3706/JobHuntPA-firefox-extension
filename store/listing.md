# Store listing (draft): Firefox Add-ons

Drafts for Firefox Add-ons (addons.mozilla.org, AMO). Publishing is the maintainer's step; copy the parts below into
the AMO developer hub's form. The privacy statement is in [`privacy.md`](privacy.md) (the same text as in the Chrome
repository): paste it into AMO's privacy policy field, or host it and link to it. The Chrome Web Store drafts are in
the Chrome repository's `store/listing.md`; the description and the reasons for the permissions are the same.

Everything here describes version 1.3.0. Check it again when the permissions or what the extension sends change.

## Name and summary

Name: **JobHuntPA**. Add-on id (fixed, in `manifest.json`): `jobhuntpa@florian3706.github.io`.

AMO summary, at most 250 characters:

> Companion to JobHuntPA, the self-hosted job-search app. Add the job you are looking at, the jobs of a search results
> page, or a careers page to your own JobHuntPA server. No account with anyone else, no analytics, no AI key in the
> browser.

The manifest's `description` (shown in `about:addons`):

> Add the page you are viewing to your own JobHuntPA server: a job, a page of search results, or a company source.

## Category, licence, support

- Category: *Other*.
- Licence: *GNU General Public License v3.0* (the extension is `GPL-3.0-only`; the text is in [`LICENSE`](../LICENSE)).
- Homepage and support site: https://github.com/florian3706/JobHuntPA
- Source code: https://github.com/florian3706/JobHuntPA-firefox-extension
- Security problems: see *Reporting a security problem* in the [README](../README.md).
- "Requires payment, non-free services or software, or additional hardware": no. It needs JobHuntPA, which is free,
  open-source software the user installs; the description says so.

## Description

> **JobHuntPA for Firefox** is the companion add-on of [JobHuntPA](https://github.com/florian3706/JobHuntPA), a free,
> open-source job-search app that you run yourself, on your own computer or your own network. The add-on only works
> with your own JobHuntPA: it has no account, no server and no service of its own.
>
> From the page you have open you can:
>
> - **Add a job.** The add-on reads the job ad in your own browser (title, company, location, salary, employment
>   type, work mode, the date and the description) and fills in a form. You check or correct it and save it to your
>   JobHuntPA. Because the page is read where you are already signed in, ads behind a sign-in work too. If the job is
>   already saved, the popup says so and lets you update it.
> - **Add the jobs on a page of search results** (SEEK for now). The popup lists the jobs on the page with tick boxes
>   and adds the ones you tick, each from what its card shows. You turn the pages yourself: the add-on never clicks,
>   scrolls or opens anything on a job site.
> - **Have a job site worked out.** On a job site the add-on doesn't know, your JobHuntPA can ask the AI provider you
>   set up in it which parts of the page hold the job's details. The popup shows you the job that layout finds, field
>   by field, and saves the layout only when you press *Use this layout*. The AI work happens in your JobHuntPA; the
>   add-on holds no AI key and talks to no AI service.
> - **Add a careers page as a source** that JobHuntPA's searches read. JobHuntPA first reads the page itself and shows
>   you the jobs it finds there before anything is saved.
>
> **What it sends, and to whom.** The add-on sends what it reads only to the JobHuntPA server address you enter, and
> only when you open its popup and use it: the job it read from the page (opening the popup on a job ad already sends
> it, to check whether JobHuntPA has it), the job cards of a results page when you press *Add jobs*, the page's
> address, the text you selected, and, when you ask JobHuntPA to work a page out, a trimmed outline of the page
> (element names, a few attributes and short samples of text). Your extension key goes with each request, to the
> server that issued it. It never sends the page's HTML, scripts, styles, cookies or anything you typed into the
> page's forms. The developer receives nothing: there are no analytics, no ads, no tracking and no third parties.
> The full privacy statement is at <privacy statement link>.
>
> **You need JobHuntPA** (free, open source, for Windows, macOS and Linux): install it, then in JobHuntPA open
> Settings, Browser extension, Connect a browser extension, and paste the server address and key it shows into the
> add-on's options page. Firefox 140 or newer; Firefox for Android 142 or newer.
>
> Open source under the GNU General Public License, version 3 (GPL-3.0-only). Source code:
> https://github.com/florian3706/JobHuntPA-firefox-extension

## Data collection and transmission (`data_collection_permissions`)

AMO's policy counts as data transmission anything "collected, used, transferred, shared, or handled outside of the
add-on or the local browser", which includes sending to a server the user runs. So the manifest declares, as
**required** (the add-on does nothing useful without them; Firefox shows them in the installation prompt and the user
accepts them by installing):

```json
"data_collection_permissions": { "required": ["websiteContent", "browsingActivity", "authenticationInfo"] }
```

| Data type | What the add-on transmits, and only to the user's own JobHuntPA server |
| --- | --- |
| `websiteContent` | The job read from the page (on opening the popup on a job ad, and on *Save job*), the job cards the user ticked on a results page, the text the user selected, and the page's trimmed outline when the user asks JobHuntPA to work the page out. |
| `browsingActivity` | The address of the page being added or worked out (and its canonical address), the addresses of the ticked jobs, and a site's name (such as `jobs.example.org`) to look up whether JobHuntPA has a layout saved for it. |
| `authenticationInfo` | The extension key the user pasted, sent as the bearer token of every request, only to the JobHuntPA server that issued it. |

Nothing else is transmitted: no `technicalAndInteraction` data (no usage statistics, crash or error reports), no
location, no personal communications, no health or financial data. There is no optional data collection, so the
manifest has no `optional` list.

Until this change the manifest declared `"required": ["none"]`, on the reasoning that the data goes to the user's own
server rather than to the developer. By AMO's definition above that was wrong; the manifest now declares what is
sent. A user updating from a version that declared `none` sees the new data types when the update installs.
`authenticationInfo` is the cautious reading (the key goes only back to its issuer, but it is a credential that leaves
the browser); it matches the *Authentication information* box ticked in the Chrome Web Store form.

## Permissions, and why each is needed

The manifest asks for `activeTab`, `scripting` and `storage`, and for the optional host permissions `http://*/*` and
`https://*/*`. There is no `<all_urls>` or other host permission granted at install, no content script on every page,
no background work on pages and no remote code. 1.3.0 asks for no permission that 1.2.0 didn't.

What Firefox shows the user: `activeTab`, `scripting` and `storage` don't add a line to the installation prompt, so at
install it shows only the data the add-on transmits (above). The site permission is asked for later, when the user
presses *Connect*: Firefox then asks whether the add-on may access the user's data for that one site, the JobHuntPA
server's address.

| Permission | What it is used for |
| --- | --- |
| `activeTab` | Gives the add-on the page in the current tab, only when you open the popup on it. Used to read the job (or a results page's job cards) from that page, to build the page's trimmed outline when you ask JobHuntPA to work the page out, and to read the job with a site's saved layout. No other tab is ever read. |
| `scripting` | Runs three small functions of the add-on in that tab (`scripting.executeScript`): `readPage` (the page's address, title, a few meta values, structured job data, the job description's text, your text selection; on a results page, what each job card shows), `readOutline` (the trimmed outline, built with DOM functions; no HTML is read out) and `readMapped` (a saved layout applied with `querySelector`, `getAttribute` and `textContent`/`innerText` only). Nothing is ever injected into a page you didn't open the popup on, and nothing changes the page. |
| `storage` | Keeps the setup in `storage.local` on this device (never synced): your JobHuntPA server's address, the extension key, the chosen workspace, the list of your workspaces' names, and the key's name and expiry date. Site layouts are not kept in the browser: they live in JobHuntPA. |
| Optional host permissions `http://*/*`, `https://*/*` (`optional_host_permissions`) | Lets the add-on call your JobHuntPA server's API. The add-on asks Firefox for the **one origin you enter** (for example `http://127.0.0.1:8000/*`) when you press *Connect* (`permissions.request`, inside that click); Firefox shows the question and you allow it or not. Nothing else is ever requested. Since the address is yours (a home computer, a Raspberry Pi, a LAN address or an https name), it can't be listed in the manifest in advance. *Reset*, or connecting to another address, withdraws the permission again (`permissions.remove`). |

## Notes to reviewer

> No build step, no minification, no bundled or third-party code: the uploaded zip is the source as it is in the
> repository (`scripts/package.sh` only zips `manifest.json`, `background.js`, `lib/`, `popup/`, `options/`, `css/`,
> `icons/`, `LICENSE` and `THIRD-PARTY.md`), so no separate source upload is needed. The only network calls are in
> `lib/api.js`, to the server address the user entered (`credentials: "omit"`). Pages are read only through
> `scripting.executeScript` with the functions in `lib/capture.js`, after the user opens the popup.
>
> To try it, the add-on needs a JobHuntPA server: install JobHuntPA (https://github.com/florian3706/JobHuntPA),
> open Settings, Browser extension, Connect a browser extension, and paste the address and key into the add-on's
> options page. Then open the popup on any job ad.

## Screenshots

The Chrome repository's end-to-end tests (`tests/e2e.cjs`) write the popup's screens to `tests/out/` (the job form, a
results page, the layout review, the careers page check, the options page). They run in Chromium but show the same
popup; they show made-up test pages and a test server, so they can be used as screenshots as they are, or retaken in
Firefox the same way.
