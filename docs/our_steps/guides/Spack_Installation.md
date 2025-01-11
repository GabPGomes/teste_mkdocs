# Spack Installation

Basically following docs:

 `git clone -c feature.manyFiles=true https://github.com/spack/spack.git`

To have spack at: /export/spack

This part I will add to /etc/profile.d

```
# For bash/zsh/sh
# at /etc/profile.d/spack.sh
. /export/spack/share/spack/setup-env.sh

# For tcsh/csh
# at /etc/profile.d/spack.csh 
source /export/spack/share/spack/setup-env.csh

~~# For fish~~ we are not testing for fish
~~#. spack/share/spack/setup-env.fish~~
```

# Configurando o Spack

$ spack compiler find

entao, criar:

```
vim $SPACK_ROOT/etc/spack/packages.yaml

packages:
  all:
    compiler: [gcc@4.9.3]
```

para os modulos:

```bash
vim $SPACK_ROOT/etc/spack/modules.yaml

modules:
  default:
    enable:
      - tcl
    tcl:
      hash_length: 0
      exclude_implicits: true
      projections:
        all: "{name}/{version}"
      all:
        autoload: all
        suffixes:
          ^ucx: ucx
          ^cuda: cuda
```

Set build dir (avoid temp for space restrictions):

```bash
cat /export/spack/etc/spack/config.yaml

config:
  build_stage:
    - /scratch/$user/spack-stage
    - $tempdir/$user/spack-stage
    - $user_cache_path/stage
```

Detect ohpc provided packages:

```bash
spack external list
spack external find automake autoconf berkeley-db
```

Para regenerar os módulos:

```bash
spack module tcl refresh --delete-tree --yes
```