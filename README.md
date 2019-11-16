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
[jovyan@7126cde066ea ~]$ jupyter kernelspec list
Available kernels:
  .net-csharp         /home/jovyan/.local/share/jupyter/kernels/.net-csharp
  .net-fsharp         /home/jovyan/.local/share/jupyter/kernels/.net-fsharp
  agda                /home/jovyan/.local/share/jupyter/kernels/agda
  bash                /home/jovyan/.local/share/jupyter/kernels/bash
  clojupyter-0.2.3    /home/jovyan/.local/share/jupyter/kernels/clojupyter-0.2.3
  common-lisp         /home/jovyan/.local/share/jupyter/kernels/common-lisp
  coq                 /home/jovyan/.local/share/jupyter/kernels/coq
  go                  /home/jovyan/.local/share/jupyter/kernels/go
  haskell             /home/jovyan/.local/share/jupyter/kernels/haskell
  ielixir             /home/jovyan/.local/share/jupyter/kernels/ielixir
  java                /home/jovyan/.local/share/jupyter/kernels/java
  jslab               /home/jovyan/.local/share/jupyter/kernels/jslab
  julia-1.2           /home/jovyan/.local/share/jupyter/kernels/julia-1.2
  lua                 /home/jovyan/.local/share/jupyter/kernels/lua
  ocaml               /home/jovyan/.local/share/jupyter/kernels/ocaml
  octave              /home/jovyan/.local/share/jupyter/kernels/octave
  pari_jupyter        /home/jovyan/.local/share/jupyter/kernels/pari_jupyter
  perl6               /home/jovyan/.local/share/jupyter/kernels/perl6
  prolog              /home/jovyan/.local/share/jupyter/kernels/prolog
  racket              /home/jovyan/.local/share/jupyter/kernels/racket
  ruby                /home/jovyan/.local/share/jupyter/kernels/ruby
  rust                /home/jovyan/.local/share/jupyter/kernels/rust
  scala               /home/jovyan/.local/share/jupyter/kernels/scala
  tslab               /home/jovyan/.local/share/jupyter/kernels/tslab
  gap-4               /usr/share/jupyter/kernels/gap-4
  python3             /usr/share/jupyter/kernels/python3
  python3-jupyroot    /usr/share/jupyter/kernels/python3-jupyroot
  sagemath            /usr/share/jupyter/kernels/sagemath
```

**Proposal for new kernels are welcome!** 

However, we except kernels distributed on anaconda because anaconda breaks current kernels dependencies.
