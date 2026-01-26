- Storage considerions
    - While considering STORAGE, I assumed if Urls stay in system for lifetime, do I need a cleanup at all ? 
    - ```Lifetime” does NOT mean “forever in hot storage with no cleanup```
    - Look for opportunities of Storage Lifecycles
    - Cleanups also mean audit cleanup, cache evictions etc .
    - We can store urls in hot or cold storage based on its retrieval frequencies.
    