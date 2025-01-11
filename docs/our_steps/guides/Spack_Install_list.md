# Spack Install list

```jsx
# removing non-generic packages
spack find # take a look at what we have
spack uninstall -a target=skylake_avx512
spack uninstall -a target=cascadelake
spack uninstall -a target=x86_64_v4
```

```jsx
#UCX for MPI
~~#spack install -j 32 ucx +dm +knem +rdmacm +xpmem~~
spack install -j32 ucx@1.14.0 +dm +knem +rdmacm +xpmem +mlx5_dv +ud +ib_hw_tm +cma

# MPICH with UCX
~~spack install -j 32 mpich@4.0.2 pmi=pmi2  netmod=ucx ^ucx /3ordip7~~
spack install -j 32 mpich@4.0.2 pmi=pmi2 +slurm netmod=ucx ^ucx /biyuciw
spack install -j 32 mpich       pmi=pmi2 +slurm netmod=ucx ^ucx /biyuciw

#OPENMPI with ucx
spack install -j 32 openmpi +thread_multiple schedulers=slurm +cuda fabrics=ucx ^ucx /biyuciw
~~~~
#CUDA
spack install -j 32 cuda@11.8.0  +dev cuda@12.1.1 +dev  cuda@10.2.89 +dev

#VIM
spack install -j 32 vim features=huge +lua +perl +python +ruby

~~# NCDU, HTOP, TREE~~ using yum packages for these
~~spack install -j 32 ncdu htop tree~~

# GCC (recent)
spack install -j 32 gcc -bootstrap

# LLVM (recent), 15, 14
spack install -j 32 llvm llvm@15.0.7 llvm@14.0.6

# PYTHON
spack install -j32 python@3.9.6 python@3.10.10 python@3.11.2

# Graphviz
spack install -j 32 graphviz  ^python@3.10.10

#CMAKE
spack mark -x cmake

# RUST

```

and then

```jsx
spack module tcl refresh --delete-tree --yes
```