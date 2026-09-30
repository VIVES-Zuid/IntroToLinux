# Summary: Lesson 3 – History & Variables

**Concepts**
- Environment vs shell (local) variables
- Important variables: `HOME`, `USER`, `PWD`, `PATH`, `PS1`, `SHELL`
- Creating (`VAR=value`) and exporting variables
- Why env vars: no hardcoded credentials
- Command history & history expansion
- Command line editing shortcuts
- `PATH` & command resolution (builtin vs executable)
- Configuration files (`~/.bashrc`, `~/.bash_profile`, …)
- Special variables `$?`, `$$`; variable manipulation `${#VAR}`, `${VAR:0:5}`, `${VAR:-default}`

**Commands**
- Variables: `env`, `set`, `echo $VAR`, `export`
- History: `history` (`-c`), `!!`, `!n`, `!text`, `!$`, `Ctrl+R`
- Editing: `Ctrl+A/E/W/K/U/Y`, `Ctrl+C/Z/L`, `Alt+F/B`
- PATH: `which`, `type` (`-a`)
- Config: `source` / `.`, `alias`
