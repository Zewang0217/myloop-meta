# myLoop — meta repository

Version-locking entry point for the myLoop delivery system. Submodules pin the
exact commit each component was verified against; `git submodule update
--init` reproduces the full tree.

## Layout

| Path | Repo | Role |
|---|---|---|
| `plugins/` | [Zewang0217/myloop-plugins](https://github.com/Zewang0217/myloop-plugins) | All Cordis plugins (discovery, deliver, review, learn, loopx, orchestrator) + tick driver |
| `core/` | [Zewang0217/myLoop](https://github.com/Zewang0217/myLoop) | ADRs, retrospectives, issue tracking, review reports |
| `dsh/` | [Zewang0217/deepseek-harness](https://github.com/Zewang0217/deepseek-harness) | dsh fork — upstream baseline `47f9438` + our fix branch |

## Local development layout

The live working tree does NOT live under this meta repo — development
happens at the original absolute paths the configs hard-code:

```
~/workspace/myloop-plugins   ← plugins (link: deps into ~/workspace/dsh)
~/workspace/myLoop           ← docs/ADRs
~/workspace/dsh              ← dsh clean clone (upgrade = git pull)
```

This meta repo exists for version locking and one-command reproduction only;
do not build here.

**New-machine setup** (clones all three + installs profile + applies the dsh
patch, idempotent):

```bash
bash <(curl -fsSL https://raw.githubusercontent.com/Zewang0217/myloop-plugins/master/scripts/bootstrap.sh)
```

## dsh fix (upstream PR in progress)

`plugins/patches/dsh-translate-empty-string-toolcall.patch` records the
DashScope-compatible tool-call fix. It lives on the fork branch
`fix/llm-deepseek-empty-string-tool-name` (commit `98f2994`) and is pending
upstream PR at deepseek-ai/deepseek-harness (organization restricts external
PR creation via API; file manually or wait for the org to open the channel).

Apply locally when running from source:

```bash
cd ~/workspace/dsh && git apply ../myloop-plugins/patches/dsh-translate-empty-string-toolcall.patch
```

## Reproduce

```bash
git clone https://github.com/Zewang0217/myloop.git
git submodule update --init --recursive
```
