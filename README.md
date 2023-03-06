# READERS_WRITERS-PROBLEM
Starve free readers-Writers problem

//problem statement - The Readers-Writers Problem is a classical synchronization problem in computer science. It deals with multiple threads or processes trying to access shared resources concurrently. Starve-Free Readers-Writers Problem, we want to ensure that readers and writers are granted access to the shared resource without starvation.



Initialization:


  Int reader_in=0;   //the number of readers currently in the critical section (initialized to 0).
  Int reader_wait=0;    //the number of readers waiting to enter the critical section (initialized to 0). 
  Int writer_in=0;       //the number of writers currently in the critical section (initialized to 0).
  Int writer_wait=0;      //the number of writers waiting to enter the critical section (initialized to 0).
  Int semaphore_mutex=1;   //a binary semaphore used to protect the shared variables reader_in, reader_wait, writer_in, and writer_wait (initialized to 1).
  Int semaphore_rd=1;      // a binary semaphore used to control access to the critical section by readers (initialized to 1).
  Int semaphore_wrt=1;     // a binary semaphore used to control access to the critical section by writers (initialized to 1).

 // reader starve free
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
    SOLUTION:
    
    //Reader starve-free:

Acquire the mutex semaphore.
If there are no writers in the critical section and no writers waiting to enter the critical section, increment the reader_in variable and release the semaphore_rd semaphore.
Otherwise, increment the reader_wait variable and release the mutex semaphore.
Acquire the semaphore_rd semaphore.
Perform the reading operation.
Acquire the mutex semaphore.
Decrement the reader_in variable.
If there are no more readers in the critical section and there are writers waiting to enter the critical section, release the semaphore_wrt semaphore and allow writers to enter the critical section.
Release the mutex semaphore.


//Writer starve-free:

Acquire the mutex semaphore.
If there are no readers or writers in the critical section, increment the writer_in variable and release the semaphore_wrt semaphore.
Otherwise, increment the writer_wait variable and release the mutex semaphore.
Acquire the semaphore_wrt semaphore.
Perform the writing operation.
Acquire the mutex semaphore.
Decrement the writer_in variable.
If there are no more writers in the critical section and there are readers waiting to enter the critical section, release the semaphore_rd semaphore and allow readers to enter the critical section.
Release the mutex semaphore.    
