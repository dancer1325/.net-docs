---
title: "Windows Workflow Overview"
description: This article describes Workflow Foundation workflows, which are models that describe real-world processes.
ms.date: "03/30/2017"
ms.assetid: fc44adbe-1412-49ae-81af-0298be44aae6
---
# Windows Workflow Overview
  
* see [here](fundamental-concepts.md)

## Workflow Run-time Engine  

* Workflow Run-time Engine
  * == in-process run-time engine
  * see [here](fundamental-concepts.md#workflow-runtime-)
* workflows can be executed | ANY Windows process
  * console applications,
  * forms-based applications,
  * Windows Services,
  * ASP.NET Web sites,
  * Windows Communication Foundation (WCF) services

* Workflow components | host process

![Workflow components in the host process](./media/44c79d1d-178b-4487-87ed-3e33015a3842.gif "44c79d1d-178b-4487-87ed-3e33015a3842")  
  
  
## Interaction between Workflow Components  
  
 ![Diagram that shows how workflow components interact.](./media/overview/workflow-component-interatction.gif)  
  
* `<xref:System.Activities.WorkflowInvoker.Invoke()>`
  * == `<xref:System.Activities.WorkflowInvoker>`'s method
    * uses
      * invoke SEVERAL workflow instances
* `<xref:System.Activities.WorkflowInvoker>`
  * uses
    * lightweight workflows / NOT need management -- from the -- host
* workflows / -- need management from the -- host
  * _Example:_ <xref:System.Activities.Bookmark> resumption
  * -- MUST be executed via -- `<xref:System.Activities.WorkflowApplication.Run()>`
* 👀if you want to invoke ANOTHER workflow instance x-> NOT required to wait for completing PREVIOUS workflow instance 👀
  * Reason: 🧠runtime engine -- supports -- running MULTIPLE workflow instances SIMULTANEOUSLY 🧠  
* _Example:_ PREVIOUS image
  * workflows invoke
    * TODO:<xref:System.Activities.Statements.Sequence> activity that contains a <xref:System.Activities.Statements.WriteLine> child activity. A <xref:System.Activities.Variable> of the parent activity is bound to an <xref:System.Activities.InArgument> of the child activity. For more information about on variables, arguments, and binding, see [Variables and Arguments](variables-and-arguments.md).
    * custom activity called `ReadLine`. An <xref:System.Activities.OutArgument> of the `ReadLine` activity is returned to the calling <xref:System.Activities.WorkflowInvoker.Invoke%2A> method.
    * custom activity that derives from the <xref:System.Activities.CodeActivity> abstract class. The <xref:System.Activities.CodeActivity> can access run-time features (such as tracking and properties) using the <xref:System.Activities.CodeActivityContext> that is available as a parameter of the <xref:System.Activities.CodeActivity.Execute%2A> method. For more information about these run-time features, see [Workflow Tracking and Tracing](workflow-tracking-and-tracing.md) and [Workflow Execution Properties](workflow-execution-properties.md).  
  
## See also

- [BizTalk Server 2006 or WF? Choosing the Right Workflow Tool for Your Project](/previous-versions/dotnet/articles/cc303238(v=msdn.10))
