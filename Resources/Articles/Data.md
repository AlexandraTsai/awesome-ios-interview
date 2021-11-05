* [Data](#data)
  * [How does NSManagedObjectContext work?](#how-does-nsmanagedobjectcontext-work)
  * [NSFetchedResultsController](#nsfetchedresultscontroller)
  * [NSPersistentContainer](#nspersistentcontainer)
  * [Managed object context and the functionality that it provides](#managed-object-context-and-the-functionality-that-it-provides)
  * [What is NSFetchRequest?](#what-is-nsfetchrequest)  


# Data

## Managed object context and the functionality that it provides
A managed object context (represented by an instance of NSManagedObjectContext) is basically a temporary “scratch pad” in an application for a (presumably) related collection of objects. These objects collectively represent an internally consistent view of one or more persistent stores. A single managed object instance exists in one and only one context, but multiple copies of an object can exist in different contexts.

You can think of a managed object context as an intelligent scratch pad. When you fetch objects from a persistent store, you bring temporary copies onto the scratch pad where they form an object graph (or a collection of object graphs). You can then modify those objects however you like. Unless you actually save those changes, though, the persistent store remains unchanged.

Key functionality provided by a managed object context includes:

Life-cycle management. The context provides validation, inverse relationship handling, and undo/redo. Through a context you can retrieve or “fetch” objects from a persistent store, make changes to those objects, and then either discard the changes or commit them back to the persistent store. The context is responsible for watching for changes in its objects and maintains an undo manager so you can have finer-grained control over undo and redo. You can insert new objects and delete ones you have fetched, and commit these modifications to the persistent store.
Notifications. A context posts notifications at various points which can optionally be monitored elsewhere in your application.
Concurrency. Core Data uses thread (or serialized queue) confinement to protect managed objects and managed object contexts. In OS X v10.7 and later and iOS v5.0 and later, when you create a context you can specify the concurrency pattern with which you will use it using initWithConcurrencyType:

## How does NSManagedObjectContext work?
`Managed object context` exists for three reasons: life-cycle management, notifications, and concurrency. It allows the developer to fetch an object from a persistent store and make the necessary modifications before deciding whether to discard or commit these changes back to the persistent store. The managed object context tracks these changes and allows the developer to undo and redo changes.

## NSFetchedResultsController
`NSFetchedResultsController` is a controller, but it’s not a view controller. It has no user interface. Its purpose is to make developers’ lives easier by abstracting away much of the code needed to synchronize a table view with a data source backed by Core Data.
Set up an NSFetchedResultsController correctly, and your table will mimic its data source without you have to write more than a few lines of code.

## NSPersistentContainer
The persistent container creates and returns a container, having loaded the store for the application to it. This property is optional since there are legitimate error conditions that could cause the creation of the store to fail.

## What is NSFetchRequest?
`NSFetchRequest` is the class responsible for fetching from Core Data. Fetch requests are both powerful and flexible. You can use fetch requests to fetch a set of objects meeting the provided criteria, individual values and more.

