<!-- myco:agent-awareness -->

## Agent awareness

- Session start / any work discussion: run `myco status --prompt`
  before proposing next steps — know the board first.
- User describes new work → offer to capture it (`myco todo add`,
  `issue new`, `proposal new`). Don't wait to be asked.
- Picking up a task: `myco resume <ID>` is the briefing.
- Wrapping up: `myco handoff <ID>` anything resumable; keep statuses
  honest (claimed → in-progress → blocked/done).

<!-- /myco:agent-awareness -->

# Agent guide — kit-web-browser-iterop

Manifest-only kit repo: `kit.hnl` is a **kit.hnl v1** pure-curation
manifest (`{ name, description, license, libs }` — no plugins, no
entry/glue; see `host/project/proposals/proposal-0004-*`). There is no
code here and no gate script yet — validation is pending loader work.

## Layout

- `kit.hnl` — the manifest. `libs` entries are module names per each
  lib's `pkg.hnl` `modules:` field.

## Curated libs (verified against pkg.hnl)

| module      | repo                  | provides                                        |
|-------------|-----------------------|-------------------------------------------------|
| `js`        | lib-js                | JS interop — `call`/`invoke`/`await`, `JS` grade |
| `concurrent`| lib-concurrent        | cooperative concurrency contract (step-arrow)   |
| `concurrent-js` | lib-concurrent-js | `concurrent` backend over `js.call`/`js.construct` |
| `fetch-js`  | lib-fetch-js          | `fetch` over generated WebIDL bindings           |
| `ws_whatwg_wasm` | lib-ws-whatwg-wasm | WHATWG WebSocket surface for wasm/JS hosts      |
| `json`      | lib-json              | JSON codec + `json.parse`/`json.stringify`       |
| `json-js`   | lib-json-js           | `json` backend over the JS `JSON` global         |
| `str-js`    | lib-str-js            | `str` ports backend via `js.call`                |
| `log`       | lib-log               | leveled logging contract (levels, attrs)         |
| `log-js`    | lib-log-js            | `log` backend to `console.*` via the `js` ffi    |

## DOM bindings

DOM/HTML WebIDL specs live inside **lib-js** (`specs/dom.idl`,
`specs/html.idl`); the generated package lands in
`lib-js/handl-packages/generated/dom/dom-webidl-browser/` via tool-codegen-webidl
(consumed via dep/path imports, the lib-fetch-js pattern). It is a build
artifact of the `js` dep — no separate lib entry is listed. Specs for
`websockets`/`storage`/`console` are planned in the same place.

## Pending / known gaps

- **No JS→handl callback**: `lib-js` has call/invoke/await but no
  `on_event` registration — DOM event listeners need a host injection
  or JS-side trampoline (`lib-event-dom` is the planned follow-up).
- **`lib-json-js`** has landed as its own package (module `json-js`,
  deps `json` + `js`) and is listed above — `json` remains the portable
  contract.
- **Validation**: kit.hnl has no loader/gate yet — manifest correctness
  is by inspection against the lib repos' `pkg.hnl`.
- The proposal's `plugins: ["compile-js"]` is dropped under kit.hnl v1
  (no plugins field); `tool-compile-js` browser targets (esm/iife/
  inline) remain a host-side concern.

## License

MIT.
