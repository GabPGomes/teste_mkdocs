# Spack compatibility with ZSH

add this on top of:

```jsx
spack-completion.bash
```

```jsx
if test -n "${ZSH_VERSION:-}" ; then
    _spack_zsh_completion_init() {
      emulate -L zsh
      if test -n "${SPACK_ROOT:-}" ; then
        SELF="${SPACK_ROOT}/share/spack/spack-completion.bash"
      else
        # try to get self from zsh, $0:A doesn't work in a function
        SELF="${(%):-%x}"
      fi
      # ensure base completion support is enabled, ignore insecure directories
      autoload -U +X compinit && compinit -i
      # ensure bash compatible completion support is enabled
      autoload -U +X bashcompinit && bashcompinit
      emulate sh -c "source '$SELF'"
    }
    if ! typeset -f complete > /dev/null ; then
      _spack_zsh_completion_init "$(emulate)"
      return # stop interpreting file
    fi
  fi
```

(source: [here](https://github.com/spack/spack/issues/20551))