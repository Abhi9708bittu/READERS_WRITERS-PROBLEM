# READERS_WRITERS-PROBLEM
Starve free readers-Writers problem



Initialization


  int reader_in;   //readers inside the critical section
  int reader_wait;    //readers waiting to go into the critical section 
  int writer_in;       //writers inside the critical section
  int writer_wait;      //readers waiting to go into the critical section 
  int semaphore_mutex=1;   ////define semaphore mutex for both readers and r
  int semaphore_rd=1;      // reader lock , rd=1 means readers is free and rd=0 means lock
  int semaphore_wrt=1;     // writer lock , wrt=1 means r is free and wrt=0 means lock


 // reader starve free
  
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
   

        
// writer starve free
  
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
     
    
    
