## Context

* January 2018
* 2000 lines of Fortran 77 (or older)
  * Currently serial
    * Performant on one core
  * Needs to be parallel
    * $\sim$12x may be sufficient
    * Not necessarily beyond single node
* Example problem supplied, runs in $\sim$1 hour
* Initial projection: $\sim$3 month project for one RSE

-

## Approach

* Benchmark sample problem with variety of compilers, compiler options
  * Auto-parallel (`icc -parallel`) gives up to 4x speedup
  * Relay this back to researcher as interim improvement
* Profile code with Intel VTune and identify hotspots
  * 2&ndash;3 functions taking up bulk of runtime
* Start wrapping hot loops in OpenMP directives

-

## Things go south

* This doesn't work
  * Code segfaults
  * Code runs more slowly
  * Code gives wrong results at the end
* Project delayed
  * $\sim$1 month of work with OpenMP abandoned

-

## Next steps

* Write regression tests for all functions
* Tidy and refactor code for readability
  * Without losing ability of researcher to maintain
* Parallelise with MPI function by function
* Hand over to another team member to finalise (May 2018)
  * See [Dr Michele Mesiti's RSECon2019 talk](https://rseconuk2019.sched.com/event/OdJH/1d1-legacy-software-a-case-study-in-optimisation-of-legacy-software) for final results of work

-

## Lessons learned

* Don't believe hype around technologies
* Testing is not optional
<div style="width:60%;margin:auto;text-align:right;display:block;"><img style="width:100%;" src="images/sc2k.png" alt="Screen shot of Sim City 2000's Transport advisor altered so that they are saying &quot;YOU CAN'T CUT BACK ON TESTING! YOU WILL REGRET THIS!&quot;"><br>
&ndash;from <a href="https://deathgenerator.com">The Death Generator</a>, by <a href="https://twitter.com/foone">@foone</a></div>
* Leading a team and keeping to a timeline developing a software project is hard

-

## Bonus: Other fumbles

* Permanently killed a cluster by using `find /`
  * Talk to people! They can tell you where things are!
* `rm -rf ~/${MISTYPED_LOOP_VARIABLE}`
  * Of course I didn't have complete backups
  * Back up your data
  * Maybe use `set -eu` when doing anything complex with `rm`?
* Wrote an $O(n^2)$ matrix routine, $n>2\times10^7$, when an $O(n)$ one was available
  * Run time went from weeks to seconds
  * The solution requiring the fewest lines of Numpy/fewest intermediary variables isn't always fastest
