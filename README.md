# Understanding how it works

![alt text](img/image.png)

The println!("hey hey") runs immediately on the main thread before any spawned future is polled. Calling spawner.spawn only pushes the async block into the executor’s queue; it does not run its body right away. When executor.run begins, it dequeues and polls the future, causing “howdy!” to print, then the timer returns Poll::Pending and the future re-queues itself. After two seconds the timer thread wakes the task via its stored waker, executor.run polls it again, and “done!” is printed.