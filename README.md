# Starve_free_readers_writers_problem
Reader-Writer problem is a classical synchronization problem in which there are multiple readers and writers and they try to access a shared resource.
The objective is to ensure concurrency without any inconsistencies.
Readers can read simultaneously but writer cannot perform simultaneously with neither reader nor writer.In order words if a writer is writing no other writer should write no other reader should read.
We are ensuring concurrency with these constraints with help of semaphores.

# Global variables
 Global variables used are readers_count and writers_count which are of int datatype and both of them are initialized to 0.

# Semaphores used
 Semaphores used are readers_count_mutex, writers_count_mutex, read_token and write_token. All of them are initialized to 1.
 readers_count_mutex ensures mutual exclusion while editing the value of readers_count.
 writers_count_mutex ensures mutual exclusion while editing the value of writers_count.
 read_token tells whether reader can read or not.
 write_token tells whether writer can write or not.

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

# Writer section pseudocode
~~~cpp
wait(wrt_token);
wait(writers_count_mutex);
writers_count++;
if(writers_count==1) wait(read_token);
signal(writers_count_mutex);


//critical section


wait(writers_count_mutex);
writers_count--;
if(writers_count==0) signal(read_token);
signal(writers_count_mutex);
signal(wrt_token);
~~~
