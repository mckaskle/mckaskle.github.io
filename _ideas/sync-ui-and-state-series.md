blog post series: keeping UI in sync with state (Core Data and KVO)


# Blog post on adding MKannotation conformance to a model object
- Using class methods for setting up dependent key paths.
- Extending a model object (as opposed to subclassing)

# Blog post: ways to refresh a few when the state of an managed object changes
Heavy: NSFetchedResultsController

Possibly lighter: NotificationCenter observer for context did save or context did change. 

Would want to compare if the changes are reported:
- before the initial save
- after a change but before a subsequent (non-insert) save
- after a save
- after a delete