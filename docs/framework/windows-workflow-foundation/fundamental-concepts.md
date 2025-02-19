---
title: "Fundamental Windows Workflow Concepts"
description: This article describes some of the concepts in workflow development in the .NET Framework 4.6.1 that may be unfamiliar to some developers.
ms.date: "03/30/2017"
ms.assetid: 0e930e80-5060-45d2-8a7a-95c0690105d4
---
# Fundamental Windows Workflow Concepts

* uses
  * | [!INCLUDE[netfx_current_long](../../../includes/netfx-current-long-md.md)]  

## Workflows and Activities  

 * workflow
   * == structured collection of actions / models a real-world process 
     * workflow's steps == hierarchy of activities / 
       * topmost activity | hierarchy -- define the -- workflow itself
       * == | PREVIOUS versions, `SequentialWorkflow` & `StateMachineWorkflow`
   * a host -- interacts with the workflow run-time engine, via 
     * `<xref:System.Activities.WorkflowInvoker>`, to -- invoke the workflow
       * == method
       * wrap `<xref:System.Activities.ActivityInstance>`
     * `<xref:System.Activities.WorkflowApplication>`, to -- get explicit control | execution of 1! workflow instance
       * wrap `<xref:System.Activities.ActivityInstance>`
     * `<xref:System.ServiceModel.WorkflowServiceHost>`-- for message-based interactions | multi-instance scenarios 
       * wrap `<xref:System.Activities.ActivityInstance>`
* activity
  * -- models -- EACH workflow's action
  * == elemental units
  * == collections of OTHER activities
  * used || executed by
    * people
    * system functions
  * ways to define them
    * `<xref:System.Activities.Activity>` -- as -- class base
      * USUALLY defined -- via -- XAML
    * `<xref:System.Activities.CodeActivity>` 
      * uses, custom created activities
      * -- can use the -- runtime -- for -- data access
      * created -- via -- CLR-compliant languages (_Example:_ C#)
    * `<xref:System.Activities.NativeActivity>`
      * -- exposes the -- breadth of the workflow runtime | activity author
      * created -- via -- CLR-compliant languages (_Example:_ C#)  

## Activity Data Model  

* == built-in activity types  
  
| Type       | Description                                                          |
| ---------- | -------------------------------------------------------------------- |
| Variable   | Stores data in an activity                                          |
| Argument   | Moves data INTO and OUT of an activity                              |
| Expression | == activity / ELEVATED return value -- used in -- argument bindings |

## Workflow Runtime  

* workflow runtime
  * == environment | workflows execute
  * `<xref:System.Activities.WorkflowInvoker>`
    * simplest way -- to execute a -- workflow
    * used by the host, for  
      * -- synchronously invoke a -- workflow
      * provide input to   OR  retrieve output -- from a -- workflow
      * add extensions / used by activities
  * `<xref:System.Activities.ActivityInstance>`
    * == thread-safe proxy /
      * used by the hosts, for
        * interact with the -- runtime
        * acquire an instance -- by -- 
          * creating it or
          * loading it | instance store.  
        * -- be notified of -- instance life-cycle events
        * control workflow execution  
        * provide input to   OR  retrieve output -- from a -- workflow
        * signal a workflow continuation & pass values | workflow
        * persist workflow data.
        * add extensions / used by activities  
    * == core activity runtime 
      * Reason: 🧠 --responsible for -- activity execution 🧠
    * | application domain,
      * can exist SEVERAL, running concurrently
  * `<xref:System.Activities.ActivityContext>`
    * derived classes
      * `<xref:System.Activities.NativeActivityContext>`
      * `<xref:System.Activities.CodeActivityContext>`
    * uses
      * activities -- gain access to the -- workflow runtime environment
      * resolve arguments and variables,
      * schedule child activities  

## Services  

* messaging activities
  * -- provided by -- Workflows
  * allows
    * accessing loosely-coupled services
      * == get data INTO and OUT of a workflow
  * built | WCF  
  * uses
    * compose SEVERAL messaging activities together -- to model a -- message exchange pattern
  * see 
    * [Messaging Activities](../wcf/feature-details/messaging-activities.md)
* Workflow services
  * hosted -- via -- `<xref:System.ServiceModel.Activities.WorkflowServiceHost>` 
* see
  * [Hosting Workflow Services Overview](../wcf/feature-details/hosting-workflow-services-overview.md)
  * [Workflow Services](../wcf/feature-details/workflow-services.md)  
  
## Persistence, Unloading, and Long-Running Workflows  

* TODO:Windows Workflow simplifies the authoring of long-running reactive programs by providing:  
  
- Activities that access external input.  
  
- The ability to create <xref:System.Activities.Bookmark> objects that can be resumed by a host listener.  
  
- The ability to persist a workflow’s data and unload the workflow, and then reload and reactivate the workflow in response to the resumption of <xref:System.Activities.Bookmark> objects in a particular workflow.  
  
 A workflow continuously executes activities until there are no more activities to execute or until all currently executing activities are waiting for input. In this latter state, the workflow is idle. It is common for a host to unload workflows that have gone idle and to reload them to continue execution when a message arrives. <xref:System.ServiceModel.Activities.WorkflowServiceHost> provides functionality for this feature and provides an extensible unload policy. For blocks of execution that use volatile state data or other data that cannot be persisted, an activity can indicate to a host that it should not be persisted by using the <xref:System.Activities.NoPersistHandle>. A workflow can also explicitly persist its data to a durable storage medium by using the <xref:System.Activities.Statements.Persist> activity.
