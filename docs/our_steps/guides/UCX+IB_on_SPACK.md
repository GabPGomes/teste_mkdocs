# UCX + IB on SPACK

yum install rdma-core-devel

```jsx
spack install -j32 ucx +dm +knem +rdmacm +xpmem

spack find -v -L ucx
-- linux-rocky8-x86_64_v4 / gcc@9.4.0 ---------------------------
3ordip72bfsvxc57fzgdvenexs6tmvqa ucx@1.14.0~assertions~backtrace_detail~cma~cuda~dc~debug+dm+examples~gdrcopy~gtest+ib_hw_tm~java+knem~logging+mlx5_dv+openmp+optimizations~parameter_checking+pic~rc+rdmacm~rocm~thread_multiple~ucg+ud~verbs~vfs+xpmem build_system=autotools libs=shared,static opt=3 simd=auto
```

set pam limits in 

/etc/security/limits.conf

for headnode and compute ndoes

Sobre UCX no Sorgan:dá pra fazer

```
ucx_info -d
```

pra listar os dispositivos, deve ter vários sobre Infiniband (mlx4_0, rc_mlx, dc_mlx, etc);pra testar a conexão, dá pra fazer

```
ucx_perftest -c 0
```

no sorgan-cpu1 e

```
ucx_perftest sorgan-cpu1 -t stream_bw -H 8 -s 81920
```