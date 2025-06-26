
# Nvidia 2025-06 ISO C++ WG21 Committee Meeting Trip Report

ISO C++ Standardization Meeting for C++ 26 conlcuded last week at Sofia, Bulgaria. This was the final meeting for adding both core and library features to C++ 26 working draft. In general, the single most significant addition to the core was ```Reflection```  and this will change how we write C++ at a fundamental level.






## Papers presented and added to C++ 26 Working Draft
 
I was at the Standards Meeting in person and had co-authored two proposals for ```std::simd``` and ```std::ranges```. My focus was on library additions of ```simd```, ```bit``` and ```execution``` (parallel algorithms) features which would benefit much of the cpu backend interface with ```C++``` and solve critical problems in concurrent systems and parallel programming.
There were several proposals in ```simd``` including the major change to have its own namespace which included all library extensions. 

### [P3691 Rename std::simd namespace](https://wg21.link/P3691)

*Nvidia Authors: Bryce Lelbach, Mark Hoemmen, Ilya Burylov, Abhilash Majumder*

The idea of putting ```std::simd``` components in their own namespace, instead of in namespace std, came late in the design process.  The new namespace was originally std::simd.  Objections to the namespace being the same as the class (`std::simd::simd`) led to the namespace being changed from `std::simd` to `std::datapar` (as in, “data-parallel types and functions”).  Library Evolution Group decided to rename the namespace back from `std::datapar` to `std::simd`. The changes makes it easier for users to see that these types relate to the SIMD (Single Instruction, Multiple Data) computer hardware feature.  It also avoids redundancy in names and distinguishes a SIMD register of values (“`vec`”) from other SIMD features like bit masks.
Here is an image with co-author Matthias Kretz and LWG member Christian Trott.

<img src ="imgs/simd.jpg" />


### [P3732R0 Numeric Range Algorithms](https://isocpp.org/files/papers/P3732R0.html)

*Nvidia Authors: Bryce Lelbach, Mark Hoemmen, Abhilash Majumder*


This objective of this paper is to add `parallel` ranges support for `numeric algorithms`. [Numeric algorithms](https://eel.is/c++draft/numeric.ops) were the terminal algorithms to be made `constexpr` and there has been a significant delay in enabling parallel support for these algorithms. Mainly from parallelization point of view, this library feature would make interfacing with algorithms such as `std::reduce` with `ranges` and execution policies possible which would inturn enhance applications such as `BLAS` , `thrust` to a large extent. This is a complex paper requiring consensus from 2 C++ subgroups - `SG1 : Concurrency and Parallelism` and `SG9 : Ranges` before its admission to Library Evolution Working Group and eventually to Library. 
 
 #### A brief overview of the design discussions and consensus from SG1 : 
 SG1 agrees that users should have a way to specify identity values, though further investigation is needed to determine whether compile-time specification is necessary or if a runtime-only interface would suffice, given concerns about the performance cost of broadcasting identity values versus using compile-time known values. The group also requests adding reduce_into and transform_reduce_into functions that write reduction results to an output range, addressing user complaints about being unable to write results to device memory and enabling in-place reduction into struct members or variables. While SG1 has no objections to adding transform_* algorithms, they request separate proposals for fixing movable-box trivial copyability and general performance issues with views, with Bryce Lelbach specifically advocating for more relaxed wording to enable parallelization around operations like filters.  
 
#### A brief overview of design discussions and consensus from SG9 : 
Since our proposal also has requirements from `SG9: Ranges` from consensus point of view, Bryce and myself had an offline brainstorming discussion with Zach Laine, and later with Jonathon Muller (SG9 chairs) regarding the design discussion and approaches. The finalized review was that the numeric algorithms should not break ABI changes introduced by parallel algorithms , and also if `projections` are needed to be used. With projections, binary `transform_reduce` would have four function arguments in a row although `fold_*` algorithms do not take projections. There were some additional discussions in terms of return type for a size-1 range, and do we need `sized` or ` forward` ranges or both. On a broader level, the design looked reasonable to the reviewers. 

This would eventually be taken up in the next ISO C++ WG21 meeting at US. 

Here is me, delighted after the discussion with Zach 

<img src = "imgs/ranges.jpg" />


## Core Working Group Features for C++ 26



## Library Working Group Features for C++ 26