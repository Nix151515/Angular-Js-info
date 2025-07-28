Async Subject = emits one value when completed
Behaviour Subject = hold one value
Replay Subject = hold a number of values and emits them on new subscriptions


tap(): The tap() operator allows you to perform side effects for each value emitted by an observable without modifying the emitted values.
 It is useful for logging, debugging, or triggering actions during the stream.

 The switchMap() the operator is used for mapping each value emitted by an observable to another observable.
  It cancels the previous inner observable when a new value arrives, ensuring that only the latest inner observable is subscribed to.

MergeMap works like switch map, but doesnt cancel the previous second obs stream when the first emits again.
For fast throughput, order does not matter

of(1, 2, 3).pipe(
  mergeMap(val => interval(1000).pipe(take(2)))
).subscribe(console.log);

0 (from val 1)
0 (from val 2)
0 (from val 3)
1 (from val 1)
1 (from val 2)
1 (from val 3)



ConcatMap works like merge map, but keeps the order (waits to finish the pending stream then continues with the other emission )
When order is important

of(1, 2, 3).pipe(
  concatMap(val => interval(1000).pipe(take(2))) // each emits 0,1 with 1s delay
).subscribe(console.log);

0   (from first inner observable)
1
0   (from second inner observable)
1
0   (from third inner observable)
1



 distinctUntilChanged(): The distinctUntilChanged() operator ensures that only distinct consecutive values are emitted by an observable
