
    Detection mechanism:
Change detection occurs when Angular want to update the DOM with the model changes
1. Angular creates a view with property-dom bindings for each component.
2. Zone.js lets Angular know of async events: that probably change the model
DOM events, requests, async operations
3. Angular does not get informed which component, so for each view it checks the previous value and the new value and marks the component as dirty
4. Then Angular starts a top-down dirty check in the component tree (through all the bindings-views) and updates its views


   Change detector:
It is used at component level to mark the component as dirty (signaling to be checked by Angular)
changeDetectorRef.detach = Angular stops monitoring the component for changes

markForCheck vs detectChanges:
markForCheck marks the component as dirty and waits the next cycle (async).
detectChanges run the change detection right ahead (synchronous).

ApplicationRef.tick() -> runs detection for the entire app, calling detectChanges on all components.
If .detectChanges() is called on the root component, it has the same effect

Whenever any data bound to the view changes, Angular checks the entire component tree to see if there are any changes. It will update the UI accordingly. We can improve with OnPush.
OnPush triggers detection only when:
- @Input changes
- @Output emits
- changeDetector manually called
- eventHandler called (onClick)
- async pipe (called markForCheck)
- observable subscriptions
Not really big real-life improvements, but enforces good practices (async)






       Zone.js
Library providing support for orchestrating async operations.
Provides execution contexts, lifecycle hooks and error handling.

  Create a zone:
var zoneA = Zone.current.fork({ name: "zoneA" });
  Run a function in the zone:
zonaA.run(function() {
 console.log('in the zone')
 zonaA.wrap(externalFn())
})

  Lifecycle hooks of Zone.js
- onScheduleTask - this callback will be called before the async operation is scheduled (before sending to the browser for scheduling in the JS event loop)
- onInvokeTask - this callback will be called before the async callback is invoked.
- onHasTask - this callback will be called when the task queue’s status change between empty and not empty

var zone = Zone.current.fork({
  name: "hook",
  onScheduleTask(delegate, current, target, task) {
    console.log("schedule ZoneTask", task.type, task.source);
    return delegate.scheduleTask(target, task);
  }
})
zone.run();


     NgZone
NgZone is the zone created for Angular by Zone.js.
It can be injected in the Angular code and run the zone functions 
(.run(), .runOutsideAngular())
We can disable zone.js from angular and take care to call the detection ourselves with the detectionRefs(.tick, .detectChanges)




   Signals change detection:
 Signals are a reactive primitive that provide a way for the code to inform the templates (and other code) that the data has changed.
 When a signal's value changes, Angular's change detection automatically updates any view that reads the signal. 
 Signals remove the need for zones.

 The difference between signal-based inputs and OnPush is that signal inputs trigger the detection only on the component itself on specific nodes, OnPush triggers for all via Zone.js.
 Signals use a dependency graph behind to track all the changes.





  ExpressionChangedAfterItHasBeenCheckedError
It indicates that a value bound to the UI has changed after Angular performed its change detection check
Angular only throws this error in development mode.

This error occurs during Angular's development mode because Angular runs change detection twice for each binding:
- The first time to update the view.
- The second time to verify that no values have changed unexpectedly.
Ensure that there are no changes to the bindings in the template after change detection is run
This often means refactoring to use the correct component lifecycle hook for your use case
If you are binding to methods in the view, ensure that the invocation does not update any of the other bindings in the template..

  Common cases:
- Changing a value in ngOnChanges or ngAfterViewInit asynchronously:
ngAfterViewInit() {
  this.value = 'new value'; // ⚠️ Might trigger the error
}
- Updating a value in response to lifecycle hooks or directives after initial binding.
- Child component changes value of parent.
- Using setTimeout or other async APIs to update a bound property.

 Fixes:
- Wrap in setTimeout() to make the update in the other detection cycle
- Use .detectChanges() after the operation
- THE BEST: Refactor logic to avoid changing bindings after the view is checked.