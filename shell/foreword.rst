========
Foreword
========

We have a large collection of (mostly ksh93) shell scripts supporting
the IFS, which have grown somewhat organically to a high level of
complexity, with a variety of different styles and approaches. Although
there have been suggestions from multiple sources over the years of
rules that should be followed, these have generally been somewhat ad-hoc
and inconsistently applied, with various degrees of applicability to
modern systems.

Further, the choice of ksh93 as a "standard" shell is an increasingly
niche one, carrying risks that (a) it may not be readily available on
newer platforms (e.g. in the context of DestinE), and (b) it's likely to
be increasingly unfamiliar to new starters.

Following discussion with stakeholders, what is presented here aims to
be a coherent set of standards and guidance for shell scripts (as we
already have for example for Fortran) promoting a consistent, structured,
and modern approach that is applicable across research, development,
testing and operational environments. While they should be sufficient to
start making improvements, it is expected that they will be further
refined and extended over time.
