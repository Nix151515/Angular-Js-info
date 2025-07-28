- Regular
1. constructor
Runs when Angular instantiates the component
2. ngOnInit
Runs once, after Angular has initialized all the component's inputs.
Runs before the component template is initialized.
Can be used to update the component state based on inputs
@Input is called before this.
3. ngOnChanges
Runs everytime the component inputs are changed.
During initialization, the first ngOnChanges runs before ngOnInit.
4. ngDoCheck
Runs everytime this component is checked for changes.
Avoid this if you can, every expensive.
Used to check for state changes outside of Angular's normal change detection.
5. ngAfterContentInit
Runs once, after the component's children (the content) have been initialized.
Triggered once after Angular projects external content into the component's view.
You can use this lifecycle hook to read the results of content queries. While you can access the initialized state of these queries, attempting to change any state in this method results in an ExpressionChangedAfterItHasBeenCheckedError
6. ngAfterContentChecked
Runs everytime the component's content has been checked for changes
7. ngAfterViewInit
Runs once after the component's view has been initialized.
Runs once after Angular initializes the component's view and its children.
You can use this lifecycle hook to read the results of view queries. While you can access the initialized state of these queries, attempting to change any state in this 
8. ngAfterViewChecked
Runs everytime the component view has been checked for changes
9. ngOnDestroy
Runs once before the component is destroyed (no longer shown on the page)
Can be replaced by injecting an instance of DestroyRef and calling a callback
  constructor() {
    inject(DestroyRef).onDestroy(() => {
      console.log('UserProfile destruction');
    });
  }


Order:
@Input, ngOnChanges, ngOnInit, ngDoCheck, the rest (init and checked)

View = the component template, HTML
This includes all elements and child components that the component itself defines.

Content = refers to elements that are passed into a component from a parent, using content projection with ng-content.
