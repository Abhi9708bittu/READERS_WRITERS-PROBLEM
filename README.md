# READERS_WRITERS-PROBLEM
Starve free readers-Writers problem

PROBLEM STATEMENT - The Readers-Writers Problem is a classical synchronization problem in computer science. It deals with multiple threads or processes trying to access shared resources concurrently. Starve-Free Readers-Writers Problem, we want to ensure that readers and writers are granted access to the shared resource without starvation.



SOLUTION : The given code implements a solution to the classical Readers-Writers problem. This problem deals with multiple readers and writers accessing a shared resource concurrently. The code provides two solutions to the problem, one that is "reader-starve-free" and another that is "writer-starve-free." Both solutions ensure that the readers and writers do not starve and get fair access to the shared resource.


//INITIALISATION :

The shared variables reader_in, reader_wait, writer_in, and writer_wait are protected using the binary semaphore semaphore_mutex. The semaphore_rd and semaphore_wrt are used to control access to the critical section by readers and writers, respectively.

 ```js
  Int reader_in=0;   //the number of readers currently in the critical section (initialized to 0).
  Int reader_wait=0;    //the number of readers waiting to enter the critical section (initialized to 0). 
  Int writer_in=0;       //the number of writers currently in the critical section (initialized to 0).
  Int writer_wait=0;      //the number of writers waiting to enter the critical section (initialized to 0).
  Int semaphore_mutex=1;   //a binary semaphore used to protect the shared variables reader_in, reader_wait, writer_in, and writer_wait (initialized to 1).
  Int semaphore_rd=1;      // a binary semaphore used to control access to the critical section by readers (initialized to 1).
  Int semaphore_wrt=1;     // a binary semaphore used to control access to the critical section by writers (initialized to 1).


```
 // reader starve free
   
  The reader-starve-free solution allows multiple readers to access the critical section concurrently while ensuring that the writers get access when no more readers are waiting. The code first acquires the mutex semaphore to protect the shared variables. It then checks if no writers are waiting or readers are in the critical section. If this condition is actual, it increments the reader_in variable and releases the semaphore_rd, allowing the readers to access the essential selection. Otherwise, it increments the reader_wait variable and releases the mutex. It then waits on the semaphore_rd to gain access to the critical section. After performing the reading operation, it acquires the mutex again and decrements the reader_in variable. If this is the last reader and writers are waiting, it releases the semaphore_wrt to allow writers to access the critical section.
   
  ```js
 p(mutex);
     if(reader_in + writer_wait ==0){
      reader_in++;
      V(semaphore_rd);
     }
     else
        reader_wait++;
        V(mutex);
        P(semaphore_rd);
        /* perform reading here ..........*/
        P(mutex);  //
        reader_in--;
        if((reader_in==0)&&(writer_wait>0))  //if there is last reader
              { 
                while(writer_wait>0)
                { 
                 V(semaphore_wrt) ;
                   writer_in ++;
                   writer_wait --;     
                } 
              } 
              V(mutex);
   
```
 
    
// writer starve free

The writer-starve-free solution allows only one writer to access the critical section at a time while ensuring that the readers get access when no more writers are waiting. The code first acquires the mutex semaphore to protect the shared variables. It then checks if no readers are waiting or writers are in the critical section. If this condition is actual, it increments the writer_in variable and releases the semaphore_wrt, allowing the writer to access the essential selection. Otherwise, it increments the writer_wait variable and releases the mutex. It then waits on the semaphore_wrt to gain access to the critical section. After performing the writing operation, it acquires the mutex again and decrements the writer_in variable. If this is the last writer and readers are waiting, it releases the semaphore_rd to allow readers to access the critical section.

  ```js 
 p(mutex);
     if(writer_in + reader_wait ==0){
      writer_in++;
      V(semaphore_wrt);
     }
     else
        writer_wait++;
        V(mutex);
        P(semaphore_wrt);
        /* perform writing here ..........*/
        P(mutex);  //
        writer_in--;
        if((writer_in==0)&&(reader_wait>0))  //if there is last writer
              { 
                while(reader_wait>0)
                { 
                 V(semaphore_rd) ;
                   reader_in ++;
                   reader_wait --;     
                } 
              } 
              V(mutex);
     ```





//CORRECTNESS OF TWO STARVE FREE SOLUTIONS
Both solutions ensure that the readers and writers do not starve and get fair access to the shared resource. For a solution to be starve-free, it must satisfy the three criteria-- Mutual Exclusion, Progress, and Bounded Waiting; Here, both solutions satisfy all three criteria. However, the reader-starve-free solution may lead to a higher degree of contention among readers, and the writer-starve-free solution may lead to a higher degree of contention among writers.
