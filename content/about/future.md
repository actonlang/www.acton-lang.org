+++
+++
# Current state 

Current state, not comprehensively listed, but items of some noteworthiness:

- the basic Acton language is mostly there
  - normal language features work, you can use Acton to implement programs
  - there may be syntax changes
- compiler error messages leave quite some room for improvement
- very little documentation :/
- no standard library
  - join the movement and write a library! :)
- actor snapshotting and resumption of computation to/from the distributed database
  - a recent regression prevents it from working {{issue(id=209)}}
  - the implementation of actor migration and placement in the new C RTS is currently under way
    - this is a feature that existed in the old Acton RTS POC, so it is a sort of proven concept but needs to be reimplemented in the C RTS
- the distributed backend does not yet support persisting data to disk
  - the database and distributed queues are all in-memory only for now
  - but its distributed, so hey, dont turn off all your machines at once ;)
- not possible to live upgrade data in database
  - we will leverage the Acton type system to detect when live upgrades are possible or when data migration needs to happen
  - assist user in writing data migration code
  - need to migrate actor state & actor messages in flight!
- Acton does not yet have a garbage collector
  - you effectively can't run long lived programs today


# Future road map

This is not a promise of delivering any feature at a certain point in time. It's to give you a rough idea of what the future might hold.

- finishing touches on deactorizer component of compiler
- restore distributed RTS functionality {{issue(id=209)}}
- replacing env arg with token {{issue(id=291)}}
