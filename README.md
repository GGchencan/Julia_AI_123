<a name="logo"/>
<div align="center">
<a href="https://julialang.org/" target="_blank">
<img src="https://julialang.org/images/logo_hires.png" alt="Julia Logo" width="210" height="142"></img>
</a>
</div>



[travis-img]: https://img.shields.io/travis/JuliaLang/julia/master.svg?label=Linux+/+macOS
[appveyor-img]: https://img.shields.io/appveyor/ci/JuliaLang/julia/master.svg?label=Windows
[coveralls-img]: https://img.shields.io/coveralls/github/JuliaLang/julia/master.svg?label=coveralls
[codecov-img]: https://img.shields.io/codecov/c/github/JuliaLang/julia/master.svg?label=codecov
# JuJu
This is the first AI model implemented by Julia.

# Note ---- some packages
```julia
julia> import Pkg
julia> Pkg.add("Flux")
julia> Pkg.add("ResumableFunctions")
julia> Pkg.add("BSON")

```

  

## Note ---- how to add custom grad

- use marco `@grad` 
  
  1. a short piece of program(to real)

  ```julia
  using Flux: @grad, TrackedReal, track

  data(x::TrackedReal) = x.data
  tracker(x::TrackedReal) = x.tracker

  function f(a) = ...

  @grad f(a::Real) = f(data(a)), Δ -> (Δ * $da)
  f(a::TrackedReal) = track(f, a) 

  ```

  2. to some function like softmax, the example extracted from `Flux/tracker/array.jl`

  ```julia
  softmax(xs::TrackedArray) = track(softmax, xs)

  @grad softmax(xs) = softmax(data(xs)), Δ -> (nobacksies(:softmax, ∇softmax(data(Δ), data(xs))),)
  ```
