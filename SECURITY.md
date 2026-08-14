# Security Policy

## Scope

This is a Claude Code skill: markdown instructions plus a static HTML/SVG rendering convention. It has no server, no build step, and no runtime dependencies. The main thing worth reporting is if generated output could execute unintended script/HTML — e.g. if diff content or file paths get embedded into the diagram's SVG/HTML without escaping, that's a real injection surface even though the output is "just a diagram."

## Reporting a Vulnerability

Please use GitHub's private reporting flow rather than a public issue:

1. Go to the [Security tab](https://github.com/Chndr-3/visualize-skills/security) of this repository.
2. Click "Report a vulnerability."

If that's unavailable, open a regular issue asking for a private contact channel — don't post exploit details publicly.

## Response

This is a solo-maintained project. There's no SLA, but reports will be acknowledged and addressed as soon as reasonably possible.
