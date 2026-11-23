# kit-web-browser-iterop

Browser/client-side JS interop kit — a `kit.hnl` v1 curation manifest
(`{ name, description, license, libs }`; no plugins, no entry/glue —
see `host/project/proposals/proposal-0004-*`).

Curates the `lib-*` modules needed to call into JavaScript from handl
and back: the `js` interop surface, `fetch` and WHATWG WebSocket over
generated WebIDL bindings, JSON, and the JS `str` backend.

## Manifest

`kit.hnl`:

```hnl
{ name: "kit-web-browser-iterop",
  description: "Browser/client-side JS interop — call/invoke/await into JS, fetch and WebSocket over generated WebIDL bindings, JSON + JS str backend.",
  license: "MIT",
  libs: ["js", "concurrent", "concurrent-js", "fetch-js", "ws_whatwg_wasm", "json", "json-js", "str-js", "log", "log-js"], }
```

`libs` entries are module names per each lib's `pkg.hnl` `modules:`
field (all verified to exist — see `AGENTS.md` for the table).

## DOM bindings

DOM/HTML WebIDL lives inside **lib-js** (`specs/dom.idl`,
`specs/html.idl`); sdkgen generates
`handl-packages/generated/dom-webidl-browser/` inside lib-js, consumed
via dep/path imports (the lib-fetch-js pattern). No separate lib entry
— it travels with the `js` dep.

## Pending

- **Loader/validation**: kit.hnl v1 has no loader or gate yet — this
  repo intentionally ships no gate script; manifest correctness is by
  inspection. Validation is pending loader work.
- **JS→handl callbacks**: no `on_event`/listener registration exists in
  `lib-js` yet — DOM events and worker `onmessage` need a host
  injection or JS trampoline (`lib-event-dom` planned).
- **`lib-json-js`**: landed as its own package — module `json-js`
  (deps `json` + `js`) over the JS `JSON` global; listed above. `json`
  remains the portable codec.

## License

MIT.
