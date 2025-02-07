runtime -os 
- mirage os, library os ==unikernels==
	- imp performance
	- Reduces attack surfaces
	- no mem bugs + type safety of OCaml
> u bulid app as unikernal, and use OS as external library. widely used in satellite systems

- Shakti hardwre, oss, designed by iitm. RISC-V  processor

> Unverified Crypto HeartBleed - TLS
> Bcrypt nov - 2024 bug



### Multi core GC of OCaml
- 5.2 - has multi core suport, delay is the impossible ness of implementing GC for multicore.
- single digit latency usually expected in ocaml
- minor heap
	- uses ptr buffer, instant replacement in unknows place. makes it crazy fast.
- major heap
multi core 
- single share mem
- multiple domain of minor heap ( domain consider as 8 comaied per core, not to be compared with threads )
- 