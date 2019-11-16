# Jupyter Metropolis
🐋 A huge docker image that contains many jupyter kernels  🚀

## Description

| Name | Size | Description |
|:-----|-----:|:------------|
| metropolis-base | 16GB | Jupyter notebook, and kernels |
| metropolis | 20GB | metropolis-base, nteract, jupyterlab, and some extensions (git,latex, drawio) |

## Supported kernels
This docker image contains more than 30 kernels.

```bash
[jovyan@1ca444c5adb0 ~]$ jupyter kernelspec list
Available kernels:
  kotlin              /home/jovyan/.ipython/kernels/kotlin
  .net-csharp         /home/jovyan/.local/share/jupyter/kernels/.net-csharp
  .net-fsharp         /home/jovyan/.local/share/jupyter/kernels/.net-fsharp
  agda                /home/jovyan/.local/share/jupyter/kernels/agda
  bash                /home/jovyan/.local/share/jupyter/kernels/bash
  c                   /home/jovyan/.local/share/jupyter/kernels/c
  clojupyter-0.2.3    /home/jovyan/.local/share/jupyter/kernels/clojupyter-0.2.3
  common-lisp         /home/jovyan/.local/share/jupyter/kernels/common-lisp
  coq                 /home/jovyan/.local/share/jupyter/kernels/coq
  fortran             /home/jovyan/.local/share/jupyter/kernels/fortran
  go                  /home/jovyan/.local/share/jupyter/kernels/go
  groovy              /home/jovyan/.local/share/jupyter/kernels/groovy
  haskell             /home/jovyan/.local/share/jupyter/kernels/haskell
  ielixir             /home/jovyan/.local/share/jupyter/kernels/ielixir
  iperl               /home/jovyan/.local/share/jupyter/kernels/iperl
  java                /home/jovyan/.local/share/jupyter/kernels/java
  jslab               /home/jovyan/.local/share/jupyter/kernels/jslab
  julia-1.2           /home/jovyan/.local/share/jupyter/kernels/julia-1.2
  lua                 /home/jovyan/.local/share/jupyter/kernels/lua
  ocaml               /home/jovyan/.local/share/jupyter/kernels/ocaml
  octave              /home/jovyan/.local/share/jupyter/kernels/octave
  pari_jupyter        /home/jovyan/.local/share/jupyter/kernels/pari_jupyter
  perl6               /home/jovyan/.local/share/jupyter/kernels/perl6
  powershell          /home/jovyan/.local/share/jupyter/kernels/powershell
  prolog              /home/jovyan/.local/share/jupyter/kernels/prolog
  racket              /home/jovyan/.local/share/jupyter/kernels/racket
  ruby                /home/jovyan/.local/share/jupyter/kernels/ruby
  rust                /home/jovyan/.local/share/jupyter/kernels/rust
  scala               /home/jovyan/.local/share/jupyter/kernels/scala
  singular            /home/jovyan/.local/share/jupyter/kernels/singular
  tslab               /home/jovyan/.local/share/jupyter/kernels/tslab
  vim_kernel          /home/jovyan/.local/share/jupyter/kernels/vim_kernel
  gap-4               /usr/share/jupyter/kernels/gap-4
  ir                  /usr/share/jupyter/kernels/ir
  python3             /usr/share/jupyter/kernels/python3
  python3-jupyroot    /usr/share/jupyter/kernels/python3-jupyroot
  sagemath            /usr/share/jupyter/kernels/sagemath
```

**Proposal for new kernels are welcome!** 

However, we except kernels distributed on anaconda because anaconda breaks current kernels dependencies.
