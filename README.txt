1. Compile
  $ make
gcc -Wall -g -pedantic -c virt_clock.c
gcc -Wall -g -pedantic -c bit_vector.c
gcc -Wall -g -pedantic oss.c virt_clock.o bit_vector.o -o oss -lrt
gcc -Wall -g -pedantic user.c bit_vector.o -o user

2. Execute
If you pass verbose, as parameter, program creates extra lines in oss.log, and log for each process started.
  $ ./oss
  # ./oss verbose

3. Description
We use bitmap for storing available process blocks, and shareable resources.
  

For synchronization we use 1 + N semaphores, where N is the number of maximum running processes.
first semapore is for the shared region, rest are process semaphores, where each process, waits
for the result of his request.

Deadlock detection is done, before we start processing requests from users.
If there is a deadlock, we try to kill first deadlocked process, then repeat the deadlock check.

User processes generate up to 10 requests, then they quit, releasing everything on exit.
This can be changed on line 99 in user.c
