Structure and families
======================

Families are used to structure the tasks of the suite, providing both a visual and
execution hierarchy. It is important to use
descriptive but concise names for families. Suites should be laid out in a consistent and
logical structure, with families grouping related tasks for clarity and to simplify the
required triggers.

Standard families that most suites should include are:

  - **setup**: tasks to install the suite's software and data dependencies
  - **admin**: tasks for suite maintenance and other manually run tasks or toggles
  - **barrier**: a family to hold the next execution date for operational suites
  - **lag**: tasks that lag behind time-critical tasks, such as archiving and cleaning
  - **cancel**: tasks to clean up the suite after it has finished


- Repeat families can be split or "unrolled" to improve throughput
    - Because the next repeat of e.g. the "main" family cannot begin until the
      previous one has finished, critical-path throughput in non-real-time
      mode (e.g. catch-up, reanalysis, hindcast) may be improved by
      one of two approaches:

        - Splitting parts that run sequentially to cycle independently (e.g. an analysis and the forecast from it generating the first guess for the next one).
        - "Unrolling" the repeat so that alternate cycles can overlap. Excessive unrolling can make the suite hard to manage, but separate repeating families for e.g. 0Z and 12Z cycles when running two cycles per day is clean and effective.


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
