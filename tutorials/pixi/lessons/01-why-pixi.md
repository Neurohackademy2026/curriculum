# 01 · Why pixi?

**Time:** 1:30–1:42 (12 min)

**Goals:** after this lesson you can:

- Name the two failure modes of ad-hoc environment management: **drift** and **non-reproducibility**.
- State what pixi adds over conda/pip: per-project environments, a lock file by default, bundled tasks, and no activation step.
- Confirm your codespace works.

## The problem

You inherit a repository. It has a `requirements.txt` or an `environment.yml` sitting at the root, so you do the obvious thing: `pip install -r requirements.txt` or `conda env create -f environment.yml`. It fails to solve. Or it solves, but the code crashes on import. Or it runs, but produces different numbers than the figure in the paper. Nobody touched the file since it was committed two years ago — the world around it moved.

This isn't bad luck, it's the default outcome of conda and pip alone. Neither tool writes a lock file unless you go out of your way to create one (`pip freeze`, `conda env export`), so the "environment" a `requirements.txt` describes is really a wish, not a record. Named conda environments are typically shared and long-lived, so they mutate under you — one `pip install` in the wrong terminal and the environment your other project depends on is different tomorrow. And whether an environment is even active is invisible state: it lives in your shell, not in the project, so it's trivially easy to run the wrong Python without noticing.

"Reproducible" here has a specific, narrow meaning we'll use for the rest of this session: **same inputs plus the same environment yields the same outputs, on anyone's machine.** Not "runs for me." Not "worked in March." Bit-for-bit, on a machine that has never seen your project before.

> 💭 **Think:** Have you ever inherited (or written!) a project you couldn't get running six months later? What broke?

## What pixi does differently

| Instead of... | pixi gives you... |
| --- | --- |
| Shared, named conda environments you activate by hand | A per-project environment tied to the folder |
| Opt-in `pip freeze` / `conda env export` (easy to forget) | `pixi.lock` written automatically on every dependency change |
| Task steps buried in a README | Tasks defined in the manifest and run with `pixi run <task>` |
| `conda activate` ceremony before every command | `pixi run` — no activation, ever |
| Separate tools/files for conda-forge vs. PyPI packages | Both conda-forge and PyPI dependencies in one manifest |

## Watch: reproducibility in one command

Everyone in the room has a pre-built example project already checked out. Run this now — you don't need to understand it yet, just watch what comes back:

```bash
cd tutorials/pixi/example
pixi run analyze
```

```
samples : 100,000
estimate: 3.150080
error   : 0.008487 (0.2702%)
wrote   : pi-estimate.png
```

Everyone in the room just produced this identical output — that is the whole tutorial in one command, and by 3:00 you will have built and reproduced this yourself from scratch. Open `pi-estimate.png` from the file explorer to see the plot it wrote.

> 💭 **Think:** What information must pixi have stored for every one of us to get bit-identical numbers just now?

## Check your setup

```bash
pixi --version
```

If that doesn't print a version, raise a hand or drop a 👋 in chat — don't fall behind silently. The [landing page setup section](../README.md#before-the-session-set-up-your-codespace) has the codespace steps if you need to redo them.

**Debrief:**

- Reproducibility = manifest + lock file + task runner, working together.
- The rest of this session builds each of those three pieces from zero.

## 🚀 If you're ahead

Skim `tutorials/pixi/example/pixi.toml`. Before Lesson 02 explains it, predict what each table (`[workspace]`, `[tasks]`, `[dependencies]`) does — you'll write one from scratch next.
