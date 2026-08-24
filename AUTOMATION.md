# Automation

Advanced Auto Brightness (AAB) exposes an **opt-in** broadcast-intent interface for automation. It can receive commands such as service on/off, pause/resume, profile loading, and emergency recovery. Moreover, AAB can broadcast selected state changes.

No Tasker plugin is required.

> **Off by default.** Commands and events only work while **External Control** is enabled (`%AAB_ExternalControl = true`) under the Live Debug Info scene.
>
> There is no authentication layer, so only enable it if you intend to actually use external automation.

---

## Commands

Send a broadcast with:

```text
Action: com.tideo.aab.control
Extra:  type:<COMMAND>
```

### Supported Commands

| `type` | Effect |
| :--- | :--- |
| `SERVICE_ON` | Turn AAB on |
| `SERVICE_OFF` | Turn AAB off |
| `SERVICE_TOGGLE` | Toggle AAB |
| `PAUSE` | Pause automatic brightness control |
| `RESUME` | Resume after a pause |
| `REAPPLY` | Recalculate and apply brightness now |
| `PANIC` | Run emergency recovery |
| `LOAD_PROFILE` | Load a profile; also pass `name:<profile>` |
| `CONTEXTS_RESUME` | Return profile selection to Context Automation |

> **Notes:**
> - `RESUME` does not override the master switch. If AAB was deliberately turned off, use `SERVICE_ON`.
> - `PANIC` uses the same recovery path as AAB's emergency gesture and remains available regardless of current service state, provided External Control is enabled.

---

### Tasker Example

Use **Action** → **System** → **Send Intent**:

```text
Action: com.tideo.aab.control
Extra:  type:LOAD_PROFILE
Extra:  name:Night Reading
Target: Broadcast Receiver
```

*Leave `Package` and `Class` empty.*

---

### ADB Examples

```bash
# Load a profile
adb shell am broadcast -a com.tideo.aab.control --es type LOAD_PROFILE --es name "Night Reading"
```
```bash
# Turn AAB off
adb shell am broadcast -a com.tideo.aab.control --es type SERVICE_OFF
```
```bash
# Emergency recovery
adb shell am broadcast -a com.tideo.aab.control --es type PANIC
```

---

## Events AAB Sends

While External Control is enabled, AAB broadcasts a few state changes using:

```text
Action: com.tideo.aab.event
Extra:  type:<EVENT>
```

### Current Events

| `type` | Meaning |
| :--- | :--- |
| `enabled` | AAB was enabled |
| `disabled` | AAB was disabled |
| `paused` | Brightness control entered the manual override state |

In Tasker, create **Profile** → **Event** → **System** → **Intent Received** with action:

```text
com.tideo.aab.event
```

The event is available as `%type` in the receiving task.

**Example:**

```text
If %type ~ paused
    ...
End If
```

---

## Profile Automation

To force a profile:

```text
Action: com.tideo.aab.control
Extra:  type:LOAD_PROFILE
Extra:  name:Video Streaming
```

To hand control back to AAB's own context rules:

```text
Action: com.tideo.aab.control
Extra:  type:CONTEXTS_RESUME
```

---

## Troubleshooting

If nothing happens, check:

- [ ] External Control is enabled.
- [ ] The action is exactly `com.tideo.aab.control`.
- [ ] `type` contains a supported command.
- [ ] `LOAD_PROFILE` also includes a valid `name`.
- [ ] Tasker's target is set to **Broadcast Receiver**.
- [ ] Use `SERVICE_ON`, not `RESUME`, if AAB is switched off.

*Malformed or unsupported commands are ignored rather than guessed.*
