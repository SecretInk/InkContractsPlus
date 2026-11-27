---
title: messages.yml
parent: Configuration
nav_order: 5
---

# messages.yml

Every line the plugin sends is in `messages.yml`, in [MiniMessage](https://docs.advntr.dev/minimessage/format.html). Clickable text works too: `<click:run_command:'/contract claim'><u>Claim them</u></click>`.

## Where a message shows

Start a message with one of these:

| Prefix | Shows as |
|---|---|
| *(none)* or `[chat]` | Chat, with `prefix` in front |
| `[noprefix]` | Chat, without the prefix |
| `[actionbar]` | Action bar |
| `[title]Title\|Subtitle` | Title and subtitle (timings in `config.yml` → `notifications`) |

Several lines send to several places:

```yaml
completed-contractor: |-
  [title]<green>Contract complete!|<gray>#<id> <white><title></white> paid <gold><amount></gold>
  [chat]<green><b>Contract complete!</b> <white>#<id> <title></white> paid <gold><amount></gold>.
```

An empty string (`''`) silences a message.

## Placeholders

Messages about a contract can use `<id>`, `<title>`, `<type>`, `<issuer>`, `<issuer_player>`, `<contractor>`, `<reward>`, `<time>` and `<status>`. Some messages have extra placeholders, which are shown in their default text.

## The prefix

```yaml
prefix: "<dark_gray>[<gold>Contracts</gold>]</dark_gray>"
```
