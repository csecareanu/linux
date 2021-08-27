In case you are using **promise and future** to synchronize threads, they have a lot in common with **condition variables**. But most of the time, task are the better choice.


|Criteria | Condition Variables | Tasks
|---|---|---|
|Multiple synchronization possible| Yes | No |
|Critical region | Yes | No |
|Exception handling in receiver | No | Yes |
|Spurious wakeup| Yes | No |
|Lost wakeup | Yes | No|

The **benefit of a condition variable** to a promise and future is, that you can use condition variables to synchronize threads multiple times.\
**In opposite to that, a promise** can send its notification only once. So you have to use more promise and future pairs to get the functionality of a condition variable.\
But in case you use the condition variable only for one synchronization, the condition variable is a lot more difficult to use in the right way.\
So a promise and future pair needs no shared variable and therefore no lock, they are not prone to spurious wakeups or lost wakeups. In addition to that, they can handle exceptions

Use tasks to synchronize threads:
```C++
// promiseFutureSynchronize.cpp

#include <future>
#include <iostream>
#include <utility>


void doTheWork(){
  std::cout << "Processing shared data." << std::endl;
}

void waitingForWork(std::future<void>&& fut){

    std::cout << "Worker: Waiting for work." << std::endl;
    fut.wait();
    doTheWork();
    std::cout << "Work done." << std::endl;

}

void setDataReady(std::promise<void>&& prom){

    std::cout << "Sender: Data is ready."  << std::endl;
    prom.set_value();

}

int main(){

  std::cout << std::endl;

  std::promise<void> sendReady;
  auto fut= sendReady.get_future();

  std::thread t1(waitingForWork,std::move(fut));
  std::thread t2(setDataReady,std::move(sendReady));

  t1.join();
  t2.join();

  std::cout << std::endl;
  
}
```
