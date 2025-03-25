# TODOs


## Problems
- because the recommendations are HPC/architecture agnostic, they are often not tangible and practical, not directly useful,
  and leave room for interpretation and implementation.
- supposed to go into the open-source ifs-arpege-coding-standards document, but scope is only for ECMWF suites, hence
  security-relevant info needs to left out.
- difficult to enforce or check.


## Ideas
- should we have a section/subsection on suite testing to advocate suite build testing as well as integration testing with the suite?

## Notes from Andrew B.

### General comments
- There is a little bit too much explanation about suites and what they are (e.g. introductions to section 1 and section 4). This should be the job of the ecFlow documentation, to which we can refer the reader.
- We should set the standard with this documentation, rather than describing the status quo, so section 4.1 could be more directive. For instance, "the main time-critical tasks of the suite should be in a family called 'main'" rather than "there is no accepted standard here, but main is often used". Another example is "setup/make/init" could be just one of these rather than a choice (unless the have a different purpose, in which case, define this). In the software section I think we unambiguously state that suites should be built using pyflow (or something which uses pyflow), rather than just recommending it. There are other examples in different sections.
- As you already mentioned, the prose could do with smoothing out in places.
- I think we should avoid linking to very specific pages of external documentation (e.g. the events/triggers etc pages of the ecFlow docs, in section 8) and instead link to the main home page of the documentation. This makes the links less prone to break and be more maintainable.
- Perhaps there should be a top-level section specific to operational considerations?

### Documentation
- Section 3.2 on the operators perspective is I think quite long, and has screenshots of dashboards which I think are overkill. Just making the point that suites need to be integrated with dashboards and specific example(s) of how that could be achieved is all that's needed here. It could probably be merged with 3.1.1
- There isn't much difference regarding the source of required documentation between 3.1.2 and 3.1.3. I think this could be enhanced a bit. For example sphinx/readthedocs style API documentation for developers.
- The whole 3.1 section I think needs a bit more detail about the level of expertise of the audience e.g. operators only use GUIs and not the command-line etc.

### Structure
- Section 4.2 should mention the ahead and behind trigger mechanism as used in RD experiment suites (e.g. the main family is prevented from running more than a few cycles ahead of the lag family to prevent a build-up of data on disk, but this is configurable via a variable). I think this is mentioned in section 9 but could be clearer.
- Section 4.2 could also do with an example of the "unrolling" or split cycling mentioned, as it's quite hard to visualise
- show the relevant trigger example as part of the toggles figure, to illustrate how this works.
- In section 4.5 could also mention that serial scripts should not be mixed in with a parallel task to avoid wasting HPC resources.
- is there any recommendation as to the placement or naming of limits in the suite? For instance, the eps_nemo RD suites have a defstatus complete family called "limits" that contains all the limits of the suite.

### Tasks
- Section 5.1: would be good to explain why tasks should take less than an hour to run? (This also doesn't align with 2 hours mentioned in section 9)
- Section 5.2: not sure idempotent is quite the right word to use here, and I think it could be removed or replaced with something more plain english.
- 5.4: I think the link to IFS scripts standards should be noted as guidance for all suites, not just IFS.

### Software dependencies
- 6.1: I think this section is a bit unclear, and perhaps needs more context:
- I don't agree with "For operational suites, local deployment ... should be minimised." How does this enable isolated testing?
- Last paragraph: is this in relation to testing? Otherwise it doesn't quite make sense
- 6.3: would be good to explain why compilation should be avoided in the same task as running

### Data management
- 7.1: "Clean working directories before you start the task" is not necessarily compatible with the earlier re-runability section

### External dependencies
- 8.5: the last link is to a user-specific confluence page. We should incorporate this text into this documentation or the ecFlow docs instead.

### Time criticality
- Section 9 repeats a lot of what was in the previous sections, and I wonder if it is even needed? It is useful to define example of time-critical tasks though, and the impact of filling up the disk because families that deal with initial conditions/driving data and archiving run too far ahead/behind the main family.
