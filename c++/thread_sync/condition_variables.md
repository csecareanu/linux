
## Lost Wakeup and Spurious Wakeup
### Lost wakeup
The phenomenon of the lost wakeup is that the sender sends its notification before the receiver gets to its wait state. The consequence is that the notification is lost. The C++ standard describes condition variables as a simultaneous synchronisation mechanism: "The condition_variable class is a synchronisation primitive that can be used to block a thread, or multiple threads at the same time, ...".
### Spurious wakeup
It may happen that the receiver wakes up, although no notification happened. At a minimum POSIX Threads and the Windows API can be victims of these phenomena.

## The wait workflow 
In the initial processing of `wait`, the thread locks the mutex and then checks the predicate `[]{ return dataReady; }`.

If the call of the predicated evaluates to
* true: the thread continues its work.
* false: condVar.wait() unlocks the mutex and puts the thread in a waiting (blocking) state

If the condition_variable `condVar` is in the waiting state and gets a notification or a spurious wakeup the following steps happen:
* The thread is unblocked and will reacquire the lock on the mutex. 
* The thread checks the predicate.
* If the call of the predicated evaluates to
  * `true`: the thread continues its work.
  * `false: condVar.wait()` unlocks the mutex and puts the thread in a waiting (blocking) state.

## Waiting for a Condition Variable
```c++
// conditionVariables.cpp

#include <condition_variable>
#include <iostream>
#include <thread>

std::mutex mutex_;
std::condition_variable condVar; 

bool dataReady{false};

void waitingForWork(){
    std::cout << "Waiting " << std::endl;
    std::unique_lock<std::mutex> lck(mutex_);
    condVar.wait(lck, []{ return dataReady; });   // (4)
    std::cout << "Running " << std::endl;
}

void setDataReady(){
    {
        std::lock_guard<std::mutex> lck(mutex_);
        dataReady = true;
    }
    std::cout << "Data prepared" << std::endl;
    condVar.notify_one();                        // (3)
}

int main(){
    
  std::cout << std::endl;

  std::thread t1(waitingForWork);               // (1)
  std::thread t2(setDataReady);                 // (2)

  t1.join();
  t2.join();
  
  std::cout << std::endl;
  
}
```
