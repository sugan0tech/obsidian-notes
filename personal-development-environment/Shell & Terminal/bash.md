### stricter bash scripts

```bash
#!/bin/bash
(dos-make-addr-conf | tee config.toml) && dosctl set template_vars config.toml
# actualy buggy script occured in Cloudflare major outage
```

- if the first element of pipe failed, pipe will still proceed
- so use `set -euo pipefail`

> [!info]
> Sometimes a programming language has a "strict mode" to restrict unsafe constructs. E.g., Perl has use strict, Javascript has
"use strict", and Visual Basic has Option Strict. But what about bash? Well, bash doesn't have a strict mode as such, but it does have an unofficial strict mode:

