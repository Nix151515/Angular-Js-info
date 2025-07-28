Hot = Observable emits from the beginning, are the same stream of data
Example: real-time communication, Subjects


Cold = Emits only when subscribed to. Does not use the same stream.
Example: Http, changes in user input

Feature	Observable VS Promise
Multiple Values	
    Can emit multiple values over time.
	Emits a single value (resolved or rejected).
Lazy Execution
	Starts only when subscribed.
	Starts immediately when defined.
Cancelation
	Supports cancelation via unsubscribe().
	Cannot be canceled once started.
Operators
	Supports powerful operators for transformation.
	Limited functionality.

