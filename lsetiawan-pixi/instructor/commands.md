# Instructor commands during exercises



## Jupyterhub note on using pixi as env

If you're using pixi-kernel on JupyterHub and cannot access the environment where JupyterLab is installed, you can use the following workaround:

1. Install `pixi-kernel` locally: `pip install pixi-kernel --user`
2. Restart your JupyterLab server
3. Check that pixi-kernel is successfully enabled: `jupyter labextension list`
4. Create any notebook underneath the directory with `pixi.toml`
  1. select the `Python (Pixi)` kernel in the notebook
  2. go to the pixi extension window (it should detect the pixi envs)
  3. select the env, save, and restart kernel



## Slide 20

*Add one more dependency of your choice from conda-forge (ideas: rich, scipy, polars) and prove you can import it. Then look at what changed in pixi.toml vs pixi.lock.*

```
# before adding the dependency
cp pixi.toml pixi.lock /tmp/

pixi add rich
pixi run python -c "import rich; rich.get_console().print('[bold green]it works[/]')"

# the intent change — one colored line
git diff --no-index /tmp/pixi.toml pixi.toml

# the lock — show the scale first, then a taste
wc -l /tmp/pixi.lock pixi.lock
diff /tmp/pixi.lock pixi.lock | head -n 15
```



## Slide 30

*Create a task full that runs the complete analysis — but only after quick has passed as a fast sanity check (task chaining).*

```
pixi task add full "python analyze.py" --depends-on quick
pixi run full
```



## Slide 38

*Create a repl feature and environment containing ipython, launch it, and prove it can see the project's packages. Then confirm the **default** environment can't.*

```
pixi add --feature repl ipython
pixi workspace environment add repl --feature repl
pixi run -e repl ipython
```



## Slide 46

*Give contributors a one-command test entry point: a test task that lives in the test feature.*

```
pixi task add test "pytest -q" --feature test
pixi run test
```

