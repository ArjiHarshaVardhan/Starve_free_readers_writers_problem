# Starve_free_readers_writers_problem
Reader-Writer problem is a classical synchronization problem in which there are multiple readers and writers and they try to access a shared resource.
The objective is to ensure concurrency without any inconsistencies.
Readers can read simultaneously but writer cannot perform simultaneously with neither reader nor writer.In order words if a writer is writing no other writer should write no other reader should read.
We are ensuring concurrency with these constraints with help of semaphores.
We are ensuring that there is no starvation which means no waiter waits indifinitely and no reader waits indifinitely under any circumstances.

# Global variables
 Global variables used are `readers_count` and `writers_count` which are of `int` datatype and both of them are initialized to 0.
 ~~~ cpp
 int readers_count=0;
 int writers_count=0;
 ~~~

# Semaphores used
   Semaphores used are `readers_count_mutex`, `read_token` and `write_token`. All of them are initialized to 1.
 
 ``` cpp
 semaphore readers_count_mutex;
 semaphore read_token;
 semaphore write_token;
 ```

 
   `readers_count_mutex` ensures mutual exclusion while editing the value of `readers_count`.
   `read_token` tells whether reader can read or not.
   `write_token` tells whether writer can write or not.

# Reader section pseudocode
~~~ cpp
wait(read_token);
wait(readers_count_mutex);
readers_count++;
if(readers_count==1) wait(wrt_token);
signal(readers_count_mutex);
signal(read_token);


//critical section


wait(readers_count_mutex);
readers_count--;
if(readers_count==0) signal(wrt_token);
signal(readers_count_mutex);
~~~

# Reader section logic
In order to execute reading it waits for `read_token` semaphore. After holding `read_token` semaphore it increments `readers_count` by holding `readers_count_mutex` before incrementing and then releases the semaphore. If now if `readers_count` is 1 which means this is first reading after one or more writings so now `wrt_token`  is held so that no writing is now executing. Now `read_token` is released before the critical section execution because reading can be done concurrently. Now critical section executes and readers_count is decremented by holding `readers_count_mutex` before decrementing and then releases the semaphore. If now if `readers_count` is 0 which means this is the last reading process before one or more writings process.

# Writer section pseudocode
~~~cpp
wait(wrt_token);
writers_count++;
if(writers_count==1) wait(read_token);


//critical section


writers_count--;
if(writers_count==0) signal(read_token);
signal(wrt_token);
~~~

# Writer section logic
In order to execute reading it waits for `write_token` semaphore. After holding `write_token` semaphore it increments `writers_count`. If now if `writers_count` is 1 which means this is first writing after one or more readings so now `wrt_token`  is held so that no reading is now executing. Now critical section executes. If now if `writers_count` is 0 which means this is the last writing process before one or more writings process. Now `write_token` is released so that another writer can start execution.
