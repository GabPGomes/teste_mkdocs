# NVIDIA missing in GPU nodes

step 1:

remove old kernels from headnode

```jsx
dnf remove $(dnf repoquery --installonly --latest-limit=-1)
```