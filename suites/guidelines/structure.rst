Structure
---------

- Suites should be laid out in a consistent and logical structure.
    - Families should be used to group related tasks, both for clarity and to
      simplify the required triggers.


- A typical suite that runs over a range of dates might consist of:
    - A "setup" or "make" family: runs once to compile sources, prepare
      invariant input data etc.
    - A "fetch", "prepare" or "initial data" family: runs with a repeat over
      dates to fetch and prepare input data.
        - May have external triggers for upstream data availability.
    - A "main" family: runs with a repeat over dates to do the main
      computational work (analysis, forecast etc.)
    - A "lag" family: runs with a repeat over dates to do non-time-critical
      post-processing (archiving outputs, offline diagnostics) and cleanup.
    - A "cancel" family: runs once when everything is finished for all dates,
      to cleanly remove the suite from ecFlow.
        - This is typically found in research suites rather than operational
          ones.


- Repeat families can be split or "unrolled" to improve throughput
    - Because the next repeat of e.g. the "main" family cannot begin until the
      previous one has finished, critical-path throughput in non-real-time
      mode (e.g. catch-up, reanalysis, hindcast) may be improved by
      one of two approaches:
        - Splitting parts that run sequentially to cycle independently (e.g.
          an analysis and the forecast from it generating the first guess for
          the next one).
        - "Unrolling" the repeat so that alternate cycles can overlap.
          Excessive unrolling can make the suite hard to manage, but separate
          repeating families for e.g. 0Z and 12Z cycles when running two
          cycles per day is clean and effective.


- Manually-run administrative tasks should be included in the suite where appropriate.
    - These should be ``defstatus complete`` so that they only run when manually
      executed.
    - Other tasks may nevertheless have triggers on them, if they need to wait
      in the event that they `are` being run.
    - They may occur either within the repeating families, or in the "setup" family or a
      dedicated "admin" family, depending on their scope.
    - Examples of such tasks might be:
        - Remove output files that would otherwise be reused rather than regenerated.
        - Apply a workaround for a known failure type.


- Configuration that may need changing "on the fly" should be done with ecFlow variables.
    - This avoids the need to "hack" deployed scripts (either on the
      filesystem, or via the ecFlow "Edit" tab)  for common on-the-fly changes
      and workarounds.
    - These should be defined at the highest relevant level in the suite and
      inherited - making it easy to change suite-wide, but still possible to
      override on individual families or tasks when necessary.
    - For boolean configuration switches, it may be convenient to use ecFlow
      events on a ``defstatus complete`` task as makeshift toggles.


- Triggers should be kept as simple as possible while ensuring the required sequencing and timeliness.
    - Where possible, triggers between separate families should be at the
      family level, with one family waiting for another to complete, rather
      than relying on detailed knowledge of individual tasks within another.
    - Exceptions to this are likely to be required in some cases, to ensure
      that the critical path is kept as short as possible by starting `parts` of
      one family as soon as `those parts of the other it actually depends on`
      have completed.
    - Nevertheless, such optimisations increase complexity, and should not be
      applied unnecessarily off the critical path.


- Limits should be used as necessary to prevent overloading HPC, ecFlow and other resources.
    - Where a suite can potentially submit a large number of tasks at once,
      but this is not essential for timeliness or throughput on the critical
      path, ecFlow limits should be used to throttle the number which are
      submitted or running at any one time.
    - This is preferable to artificially `sequencing` independent tasks with
      triggers, as it allows flexible control of the level of parallelism.
    - This is particularly important for tasks that are off the critical path
      and rely on "bottleneck" services like archivng.
