# SPAN + AVISPA: Key Exchange Through a Trusted Server

**A university project modeling a session-key exchange protocol in HLPSL and analyzing it with SPAN/AVISPA.**

[الشرح بالعربية](docs/PROJECT_EXPLANATION_AR.md) · [تقرير المشروع بصيغة Word](docs/PROJECT_REPORT_AR.docx)

## Overview

Alice (A) creates a fresh session key for communication with Bob (B). A trusted server (T) forwards the key to Bob using the long-term symmetric keys that it shares with each participant. Bob then challenges Alice with a fresh nonce to check that she can use the session key.

The standalone specification is [`protocol.hlpsl`](protocol.hlpsl). A cleaned Arabic report is included as optional documentation. The supplied original reports remain local and are excluded from the GitHub upload.

## Protocol flow

The notation `{message}_key` means that the message is encrypted with the named symmetric key.

![Protocol message flow](docs/evidence/protocol-sequence.png)

1. **A → T:** `{B, Kab}_Kat` — Alice creates `Kab` and identifies Bob as its intended peer.
2. **T → B:** `{A, Kab}_Kbt` — the trusted server forwards the key and identifies Alice as its source.
3. **B → A:** `{B, Nb}_Kab` — Bob sends a fresh challenge nonce `Nb` under the session key.
4. **A → B:** `{Nb}_Kab` — Alice returns the challenge under the same key.

`Kat` is the long-term key shared by A and T; `Kbt` is shared by B and T; `Kab` is the session key; `kit` is the intruder's own key.

## What the model checks

- **Secrecy:** `Kab` is declared secret among A, B, and the trusted server T.
- **Key agreement authentication (`auth_1`):** Alice requests authentication on `Kab`; Bob supplies the matching witness event.
- **Challenge authentication (`auth_2`):** Bob requests authentication on `Nb`; Alice supplies the matching witness event when she returns it.
- **Attacker scenarios:** the environment includes two concurrent honest sessions, a session where the intruder impersonates A to Bob, and one where Alice's peer is the intruder. Channels use the Dolev–Yao attacker model.

The model assumes ideal symbolic encryption, uncompromised long-term keys, and a trusted server. A symbolic model alone cannot establish that a real implementation is secure against implementation bugs or threats outside the model.

## Verification status

This repository documents the protocol and its security goals; it does not claim a verification outcome. The original HLPSL file was not supplied separately, so `protocol.hlpsl` was reconstructed from the full-code listing in the report. Run this file in SPAN/AVISPA and record the tool version, backend, settings, and output before reporting an analysis result.

## Analyze the specification

Open `protocol.hlpsl` in an AVISPA environment and select an analysis backend. SPAN can display and animate HLPSL protocol runs. See the [AVISPA User Manual](https://avispa-project.org/package/user-manual.pdf) and [HLPSL Tutorial](https://avispa-project.org/package/tutorial.pdf) for tool setup and usage.

## Repository contents

```text
protocol.hlpsl                  Standalone HLPSL model
docs/PROJECT_EXPLANATION_AR.md Arabic project explanation
docs/PROJECT_REPORT_AR.docx     Cleaned Arabic project report
docs/evidence/                  Protocol message-flow diagram
```

The supplied Word and PDF reports are kept in `docs/original/` on this computer and excluded from the GitHub upload.

## Upload to GitHub

Create an empty repository on GitHub, then run these commands from this project folder. Replace the remote URL with your repository's URL.

```bash
git add .
git commit -m "Add AVISPA key exchange project"
git branch -M main
git remote add origin https://github.com/USERNAME/REPOSITORY.git
git push -u origin main
```

No license has been added yet. Choose one before inviting others to reuse or redistribute the project.
