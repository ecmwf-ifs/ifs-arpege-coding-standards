Tasks
-----

- A task should perform one function only
- All tasks should be rerunnable
    - Rerunning  tasks should  produce identical outputs for the same input
    - Each task run should  be independent of any previous run
    - Use temporary  directories and  remove when finished
    - Copy data needed  for later tasks to shared working  directory
    - If this is not possible, clear instructions on how to rerun should  be in the manual  doc

- Be careful with error checking and reporting
    - Use meaningful and consistent error messages
    - KSH functions can mess up error traps

- Don’t do serial things in parallel tasks
- Check task run times
    - ideally no task should take more than 1 hour

- Use HPC (and ecFlow) resources efficiently