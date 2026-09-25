---
title: "Module 3 · HTTP & Chrome DevTools"
bookToc: false
---

# Module 3: HTTP & Chrome DevTools

Your browser talks to servers through requests and responses. You will inspect this conversation with curl and Chrome, then make a temporary visual edit to a page.

> Have Chrome and curl ready. Use harmless practice text; the public httpbin service can see what you send.

## Hey, Let’s Play the Game!

> “Since all models are wrong the scientist cannot obtain a ‘correct’ one by excessive elaboration.” — [George Box, “Science and Statistics”](https://doi.org/10.1080/01621459.1976.10480949)

| 🎮 **Find a clue. Unlock the next level.** |
| --- |
| Play **[OverTheWire: Natas](https://overthewire.org/wargames/natas/)** for real web puzzles. Use **[httpbin](https://httpbin.org/)** alongside it to experiment with requests. Start after the HTTP lessons. |

<details>
<summary><strong>🎮 OverTheWire: Natas · Unlock the next web level</strong></summary>

## Natas · Follow the web clues

| Play card | Your starting point |
| --- | --- |
| **Choose this when** | You want a web-security puzzle after practising HTTP |
| **Setup** | Browser and DevTools; no installation or personal account |
| **Access** | Free hosted wargame |
| **First win** | Solve Level 0; levels 0–10 are an optional stretch |

### Why this is a game

Each level hides the credential for the next. Inspect pages, responses, and clues to progress. This is web-security practice, so later puzzles go beyond this bridge.

### Start playing

1. Read the [official Natas introduction](https://overthewire.org/wargames/natas/) and [game rules](https://overthewire.org/rules/).
2. Open [Level 0](http://natas0.natas.labs.overthewire.org/).
3. Use the published starter username **natas0** and password **natas0**.
4. Read the challenge. Open DevTools → **Network**, reload, and inspect the document response.
5. Compare what the browser displays with the HTML it received. Look for a clue before searching for a walkthrough.
6. Use your discovery to try Level 1; record what you learned privately.

### Your first win

- [ ] I explained the evidence that unlocked the next level.
- [ ] I recorded an HTTP observation without publishing a password.

Practise only on the designated game hosts. Start with Level 0; continue toward Level 10 only if you enjoy the challenge.

</details>

<details>
<summary><strong>🧪 httpbin · Experiment with requests and responses</strong></summary>

## httpbin · Your HTTP practice bench

| Practice card | Your starting point |
| --- | --- |
| **Choose this when** | You want to understand exactly what your request sends |
| **Setup** | [Open httpbin](https://httpbin.org/); use curl from Module 1 |
| **Access** | Free hosted playground; no account |
| **First win** | Explain an echoed value, a status code, and a redirect |

### Features worth trying

- **Echo endpoints** show your query parameters, headers, or submitted data.
- **Status endpoints** let you deliberately request responses such as `404`.
- **Redirects and delays** let you observe browser behaviour without needing your own server.

This is a playground, not a scored game. Use it to test an idea before returning to Natas.

### Try three experiments

Run these in your learning terminal:

```bash
curl -i 'https://httpbin.org/get?topic=bridge'
curl -i 'https://httpbin.org/headers' -H 'X-Bridge-Practice: ready'
curl -i 'https://httpbin.org/redirect/1'
curl -i -L 'https://httpbin.org/redirect/1'
```

1. Find `topic=bridge` in the first response.
2. Find your custom header in the second.
3. Compare the last two requests: `-i` displays response headers; `-L` follows the redirect.
4. Open [the same GET in Chrome](https://httpbin.org/get?topic=bridge), then inspect it in **Network**.
5. Optional: request `https://httpbin.org/status/404` and explain why it is still an HTTP response.

- [ ] I found my own data in an echoed response.
- [ ] I can explain what following a redirect changes.

Send invented practice data only. If the public service is unavailable, retry later.

[Official httpbin endpoint explorer](https://httpbin.org/)

</details>

## Learning path

Open a section to begin. Work in order, try the commands, and reveal answers only after choosing your own.

<details name="module-3">
<summary><strong>3.1 · The round trip</strong></summary>

## Learn

A **client** (browser or curl) sends a request to a **server**. The server returns a response. HTTP defines the messages; HTTPS protects their transport with encryption.

```text
You type or click
      ↓
Client → method + URL + headers + optional body → Server
Client ← status + headers + response body      ← Server
      ↓
Browser displays something, or curl prints the response
```

### Read a URL

Example: `https://httpbin.org/get?topic=python`

| Part | Value |
| --- | --- |
| Scheme | `https` |
| Host | `httpbin.org` |
| Path | `/get` |
| Query parameter | `topic=python` |

- **Endpoint:** an address an application accepts requests at.
- **API:** defines how software can use such endpoints.

### Read the messages

| Part | Example | Purpose |
| --- | --- | --- |
| Method | `GET`, `POST` | Requested kind of action |
| Request header | `Accept: application/json` | Extra information about the request |
| Request body | `{"topic":"python"}` | Data sent to the server |
| Status | `200`, `404` | Outcome of the request |
| Response header | `Content-Type: application/json` | How to interpret returned data |
| Response body | JSON, HTML, an image | Returned content |

JSON is a text format for data: objects use `{}`, keys and strings use double quotes. `{"module":3,"ready":true}` has a number and a boolean value.

## Predict

Is `404` a connection failure or a server response?

<details>
<summary>Answer</summary>

It is a response saying the requested resource was not found. A DNS lookup failure or connection timeout can happen before any HTTP response arrives.

</details>

</details>

<details name="module-3">
<summary><strong>3.2 · Send a GET</strong></summary>

## Practice

[httpbin](https://httpbin.org/) is a request/response playground. Its echo endpoints show data you sent, making them useful for experiments.

```bash
curl -i 'https://httpbin.org/get?topic=python&module=3'
```

- **Quotes:** keep `&` from being interpreted by the shell.
- **`-i`:** include response headers.

### Find three pieces of evidence

- [ ] A success status.
- [ ] A JSON content-type.
- [ ] An `args` object containing `topic` and `module` as strings.

Header capitalisation and protocol versions can vary.

**Change one thing:** replace `python` with `linux`. Find the changed value in the response rather than reading every field.

Now save just the response body:

```bash
cd ~/bridge-lab
curl -sS 'https://httpbin.org/get?topic=linux' -o notes/http-get.json
python3 -m json.tool notes/http-get.json
```

- **`-sS`:** hide the progress meter, but keep error messages.
- **`-o`:** write the response body to a file.
- **`json.tool`:** format the saved JSON.

## Check

- [ ] The formatted output has an `args` entry with `topic: linux`.

If the service returns HTML, a rate limit, or a gateway error, wait and retry later. Do not treat every returned body as JSON. Preserve the exact status and error in your notes if it persists.

</details>

<details name="module-3">
<summary><strong>3.3 · Methods and status</strong></summary>

## Practice

POST commonly submits data. This example sends a JSON body:

```bash
curl -i 'https://httpbin.org/post' -H 'Content-Type: application/json' -d '{"learner":"Explorer","module":3}'
```

`-H` adds a header; `-d` supplies the body and makes curl use POST by default. Find your object in the response’s `json` field.

Try two more actions and two deliberate status responses:

```bash
curl -i -X PUT 'https://httpbin.org/put' -H 'Content-Type: application/json' -d '{"ready":true}'
curl -i -X DELETE 'https://httpbin.org/delete'
curl -i 'https://httpbin.org/status/404'
curl -i 'https://httpbin.org/status/500'
```

`-X` sets the method explicitly.

| Method | Common purpose |
| --- | --- |
| GET | Retrieve a resource |
| POST | Submit data |
| PUT | Replace a resource |
| PATCH | Change part of a resource |
| DELETE | Request removal |

> **Practice service:** httpbin echoes/simulates behaviour; it is not permanently storing and deleting your learner record.

### Read the status

| Status family | Read it as |
| --- | --- |
| `2xx` | Request succeeded, e.g. `200` |
| `3xx` | Redirection, e.g. `302`; curl needs `-L` to follow |
| `4xx` | Request cannot be fulfilled as sent, e.g. `400`, `401`, `403`, `404` |
| `5xx` | Server-side failure, e.g. `500` |

## Check

- [ ] Both `/status/` calls return the requested status, even if the body is empty.
- [ ] I can explain why receiving a response does not always mean the request succeeded.

## Try it yourself

Use `/patch` with `-X PATCH` and a JSON field `"topic":"git"`. Record the method, status, and one echoed field in `notes/http.txt`.

</details>

<details name="module-3">
<summary><strong>3.4 · Inspect the Network tab</strong></summary>

## Practice

Start with Google’s stable [Network activity demo](https://chrome.dev/devtools-network-activity/getstarted.html).

1. Open Chrome DevTools with **Ctrl+Shift+I** on Windows/Linux or **Cmd+Option+I** on macOS. Select **Network**.
2. Reload the demo. The list records requests made while DevTools is open; each row is a resource or request.
3. Click the demo’s **Get Data** button. Find `getstarted.json`, using the filter if needed.
4. Select it. In **Headers**, find the Request URL, Request Method, and Status Code. Compare Request Headers with Response Headers.
5. Open **Response** to inspect raw data, **Preview** for its rendered form, and **Timing** for the request timeline.

## Check

- [ ] I can identify the action that triggered the JSON request.
- [ ] I found a value in its response and can explain what it represents.

If the interface looks unfamiliar, follow the [official Network tutorial](https://developer.chrome.com/docs/devtools/network).

### Repeat a browser request in your terminal

1. Open `https://httpbin.org/get?topic=browser` in a separate tab with Network open.
2. Select the document request.
3. Right-click and choose **Copy → Copy as cURL**.
4. Inspect the copied command, then run it in your terminal.

> **Share safely:** this is your own harmless practice request. Commands copied from signed-in sites may include cookies or tokens and should not be shared.

**Compare:** browser and curl can request the same endpoint without producing identical headers. A document navigation is not necessarily listed under Fetch/XHR.

</details>

<details name="module-3">
<summary><strong>3.5 · Investigate suggestions</strong></summary>

## Discovery lab

Find the request behind Google search suggestions rather than memorising an endpoint that may change.

1. Open [Google Search](https://www.google.com/) in Chrome. Complete any consent prompt.
2. Open **Network**, clear the request list, and type a harmless phrase such as `weather in ch` into the search box without submitting.
3. Try the **Fetch/XHR** filter. If empty, switch to **All**; suggestions may use a different request type or cached data. Try another phrase.
4. Inspect requests triggered as you type. A name containing `complete`, `search`, or `suggest` is a clue, not proof.
5. Compare the URL/query or payload with your typed text, then examine Preview/Response for suggestion data.

### Record your evidence in notes/http.txt

- **Where:** host and path.
- **Action and result:** method and status.
- **Input:** parameter carrying your query.
- **Connection:** why you believe the response produced the visible suggestions.

> **Keep it safe:** do not save cookies, authorisation headers, or an entire account-related request.

### If the request is hard to find

- **Expect variation:** Google’s interface depends on region, browser, session, and experiments.
- **Do not assume API support:** an internal endpoint seen in DevTools is not necessarily a supported public API.
- **If you cannot find the request:** use the earlier Get Data demo and document its request instead.

The goal is the same: identify a request from evidence.

<details>
<summary>What a useful observation sounds like</summary>

“When I typed a new phrase, a new request appeared. Its query contained that phrase, and the response included the suggestions I saw. I recorded the actual host and path from my browser.” Avoid guessing an endpoint solely from its filename.

</details>

</details>

<details name="module-3">
<summary><strong>3.6 · Change the page locally</strong></summary>

## Practice

The **Elements** panel shows the DOM: the browser’s current representation of the page. Its Styles pane lets you experiment with appearance.

1. On Google’s homepage, right-click the logo (or current doodle) and choose **Inspect**. If it is hard to select, use the element picker: Ctrl+Shift+C, or Cmd+Option+C on macOS.
2. Select the image or its visible wrapper. In the Styles pane, add `filter: grayscale(1);` and `transform: rotate(-8deg);` under `element.style`.
3. Observe the changed logo. Uncheck each property to compare before and after.
4. Reload the page. With normal settings and no local overrides, your changes disappear.

## Check

- [ ] Reloading removes my local edit.
- [ ] I can explain why editing a displayed value does not change the value stored by the site.

If a doodle or iframe makes the logo difficult to edit, practise on the [official DOM tutorial](https://developer.chrome.com/docs/devtools/dom) and its linked demo: inspect a heading, edit its text, and reload. The same browser-side principle applies.

**Tiny experiment:** disconnect from the network after a page has loaded and change a style. Does the local edit still work? Explain why inspecting/editing an existing DOM differs from fetching new content.

</details>

## Check your understanding

Revisit any lab whose evidence you cannot explain.

### 1. Where does the JSON you send with curl -d go?

- **A.** Always into the URL path
- **B.** Into the request body
- **C.** Into a Git commit

**Answer and why:** B. `-d` supplies request data. The Content-Type header tells the server you intend that body to be JSON; it does not make malformed JSON valid.

### 2. You rotate Google’s logo in Elements. Who sees that edit?

- **A.** Everyone visiting Google
- **B.** Only Google’s developers
- **C.** You, in that local page view

**Answer and why:** C. A normal DevTools edit changes the browser’s current document. It is not an authenticated update to the website’s stored content.

### 3. A response says HTTP 500. What does that tell you?

- **A.** A server returned an error response
- **B.** No server was reached at all
- **C.** The request definitely succeeded

**Answer and why:** A. You received an HTTP response; its status indicates a server-side error. It may come from the application or an intermediary such as a gateway.

## Before you continue

- [ ] I can identify method, URL, headers, body, and status.
- [ ] I sent GET and POST requests and inspected echoed data.
- [ ] I identified a browser request using observed evidence.
- [ ] I edited a page locally and explained why refreshing removed the edit.

## Explore yourself

Choose one browser or HTTP playground after this module.

- [httpbin](https://httpbin.org/) lets you experiment with request headers, redirects, status codes, and delays.
- [Chrome DevTools demos](https://chrome.dev/devtools-network-activity/getstarted.html) provide a stable page for inspecting Network activity and editing the DOM.
- [OverTheWire: Natas](https://overthewire.org/wargames/natas/) offers web-security puzzles. Start with the game card above; levels 0–10 are optional further exploration.

**Try next:** pick one endpoint or one browser action, predict the result, then record the evidence you observe.


---

**Next:** [Module 4: Git & GitHub →](../04-git-and-github/)
