# C : Matrix Multiplication

- https://www.geeksforgeeks.org/c/c-matrix-multiplication/
- https://www.programiz.com/c-programming/examples/matrix-multiplication
- https://www.programiz.com/c-programming/examples/matrix-multiplication-function
- https://en.wikipedia.org/wiki/Matrix_multiplication
- https://www.wscubetech.com/resources/c-programming/programs/matrix-multiplication
- https://www.geeksforgeeks.org/cpp/cpp-matrix-multiplication/
- https://www.codewithc.com/matrix-multiplication-in-c/?amp=1
- https://www.w3resource.com/c-programming-exercises/array/c-array-exercise-21.php#google_vignette
- https://www.scaler.com/topics/matrix-multiplication-in-c/
- https://www.startertutorials.com/blog/c-program-add-multiply-two-compatible-matrices.html
- https://docs.vultr.com/clang/examples/multiply-two-matrices-using-multi-dimensional-arrays
- https://labex.io/tutorials/c-multiply-two-matrices-in-c-435191
- https://www.sanfoundry.com/c-program-perform-matrix-multiplication/#google_vignette
- https://www.c-sharpcorner.com/article/2d-array-with-matrix-multiplication-in-c-programming/

```c
// C program to multiply two matrices
#include <stdio.h>
#include <stdlib.h>

// matrix dimensions so that we dont have to pass them as
// parametersmat1[R1][C1] and mat2[R2][C2]
#define R1 2 // number of rows in Matrix-1
#define C1 2 // number of columns in Matrix-1
#define R2 2 // number of rows in Matrix-2
#define C2 3 // number of columns in Matrix-2

void multiplyMatrix(int m1[][C1], int m2[][C2])
{
    int result[R1][C2];

    printf("Resultant Matrix is:\n");

    for (int i = 0; i < R1; i++) {
        for (int j = 0; j < C2; j++) {
            result[i][j] = 0;

            for (int k = 0; k < R2; k++) {
                result[i][j] += m1[i][k] * m2[k][j];
            }

            printf("%d\t", result[i][j]);
        }

        printf("\n");
    }
}

// Driver code
int main()
{
    // R1 = 4, C1 = 4 and R2 = 4, C2 = 4 (Update these
    // values in MACROs)
    int m1[R1][C1] = { { 1, 1 }, { 2, 2 } };

    int m2[R2][C2] = { { 1, 1, 1 }, { 2, 2, 2 } };

    // if coloumn of m1 not equal to rows of m2
    if (C1 != R2) {
        printf("The number of columns in Matrix-1 must be "
               "equal to the number of rows in "
               "Matrix-2\n");
        printf("Please update MACROs value according to "
               "your array dimension in "
               "#define section\n");

        exit(EXIT_FAILURE);
    }

    // Function call
    multiplyMatrix(m1, m2);

    return 0;
}
```

# C / C++ : Synchronization

- https://www.geeksforgeeks.org/linux-unix/mutex-lock-for-linux-thread-synchronization/
- https://en.cppreference.com/w/cpp/thread/mutex.html
- https://www.codequoi.com/en/threads-mutexes-and-concurrent-programming-in-c/
- https://en.wikipedia.org/wiki/Lock_(computer_science)
- https://www.geeksforgeeks.org/c/use-posix-semaphores-c/
- https://docs.oracle.com/cd/E19683-01/806-6867/6jfpgdcnj/index.html
- https://en.wikipedia.org/wiki/Spinlock
- https://wiki.osdev.org/Spinlock
- https://dev.to/ivinieon/features-and-differences-of-spinlock-mutex-and-semaphore-3l1m
- https://medium.com/@lakshminath_alamuru/mutex-vs-semaphore-and-its-internal-implementation-1500a9d58242
- https://www.geeksforgeeks.org/operating-systems/mutex-vs-semaphore/
- https://medium.com/@everythingismindgame/synchronization-using-condition-variable-in-c-dc4d427d1af6
- https://en.cppreference.com/w/cpp/thread/condition_variable.html
- https://www.geeksforgeeks.org/linux-unix/condition-wait-signal-multi-threading/
- https://medium.com/@jtchen2k/using-c-conditional-variables-for-thread-synchronization-87fb1ac3601c
- https://en.wikipedia.org/wiki/Monitor_(synchronization)

- https://courses.cs.washington.edu/courses/cse451/24wi/lectures/08-24wi%20Semaphores,%20Condition%20Variables%20&%20Monitors.pdf

![Image!](https://miro.medium.com/v2/resize:fit:1400/1*X0V54LQtlH0SpUvdjcsk_Q.png "Image")

```c++
#include <cstdlib>
#include <iostream>

#include <thread>
#include <mutex>
#include <condition_variable>
#include <chrono>

#define STORAGE_MIN 10
#define STORAGE_MAX 20

int storage = STORAGE_MIN;

std::mutex globalMutex;
std::condition_variable condition;

void consumer()
{
	std::cout << "[CONSUMER] thread started" << std::endl;
	int toConsume = 0;
	
	while(true) {
		std::unique_lock<std::mutex> lock(globalMutex);
		if (storage < STORAGE_MAX) {
			condition.wait(lock , []{return storage >= STORAGE_MAX;} ); // Атомарно _отпускает мьютекс_ и сразу же блокирует поток
			toConsume = storage-STORAGE_MIN;
			std::cout << "[CONSUMER] storage is maximum, consuming "
				      << toConsume << std::endl;
		}
		storage -= toConsume;
		std::cout << "[CONSUMER] storage = " << storage << std::endl;
	}
}

/* Функция потока производителя */
void producer()
{
	std::cout << "[PRODUCER] thread started" << std::endl;

	while (true) {
		std::this_thread::sleep_for(std::chrono::milliseconds(200));
		std::unique_lock<std::mutex> lock(globalMutex);
		++storage;
		std::cout << "[PRODUCER] storage = " <<  storage << std::endl;
		if (storage >= STORAGE_MAX) {
			std::cout << "[PRODUCER] storage maximum" << std::endl;
			condition.notify_one();
		}
	}
}

int main(int argc, char *argv[])
{
	std::thread thProducer(producer);
	std::thread thConsumer(consumer);
	
	thProducer.join();
	thConsumer.join();
	
	return 0;
}
```

# C / C++ : Atomic

- https://en.cppreference.com/w/cpp/atomic/atomic.html
- https://cplusplus.com/reference/atomic/
- https://www.geeksforgeeks.org/cpp/cpp-11-atomic-header/
- https://medium.com/developer-rants/c-threads-and-atomic-variables-oversimplified-b37bbbe3f2e6
- https://en.cppreference.com/w/cpp/header/atomic.html
- https://www.modernescpp.com/index.php/atomic-ref/
- https://cplusplus.com/reference/atomic/atomic/atomic/

```c++
#include <atomic>
#include <iostream>
#include <thread>
using namespace std;

atomic<int> counter(0);

void increment_counter(int id)
{
    for (int i = 0; i < 100000; ++i) {
        // Increment counter atomically
        counter.fetch_add(1);
    }
}

int main()
{
    thread t1(increment_counter, 1);
    thread t2(increment_counter, 2);

    t1.join();
    t2.join();

    cout << "Counter: " << counter.load() << std::endl;
    return 0;
}
```

# C++ : thread pool

- https://www.geeksforgeeks.org/cpp/thread-pool-in-cpp/
- https://medium.com/@bhushanrane1992/getting-started-with-c-thread-pool-b6d1102da99a
- https://nixiz.github.io/yazilim-notlari/2023/10/07/thread_pool-en
- https://matgomes.com/thread-pools-cpp-with-queues/
- https://dev.to/ish4n10/making-a-thread-pool-in-c-from-scratch-bnm
- https://arxiv.org/html/2407.15805v2
- https://blog.eiler.eu/posts/20210512/

> The Thread Pool in C++ is used to manage and efficiently resort to a group (or pool) of threads. Instead of creating threads again and again for each task and then later destroying them, what a thread pool does is it maintains a set of pre-created threads now these threads can be reused again to do many tasks concurrently. By using this approach we can minimize the overhead that costs us due to the creation and destruction of threads. This makes our application more efficient.

![Image!](https://miro.medium.com/v2/resize:fit:1100/format:webp/1*bW8heZuEzTDPpw1HmpNZmA.png "Image")
![Image!](https://miro.medium.com/v2/resize:fit:1100/format:webp/1*ZbYw8sUijxHYi3G8TmtXOw.png "Image")
![Image!](https://nixiz.github.io/yazilim-notlari/assets/img/use_future_high_order_sequence_diagram.png "Image")

```c++
#include <condition_variable>
#include <functional>
#include <iostream>
#include <mutex>
#include <queue>
#include <thread>
using namespace std;

// Class that represents a simple thread pool
class ThreadPool {
public:
    // // Constructor to creates a thread pool with given
    // number of threads
    ThreadPool(size_t num_threads
               = thread::hardware_concurrency())
    {

        // Creating worker threads
        for (size_t i = 0; i < num_threads; ++i) {
            threads_.emplace_back([this] {
                while (true) {
                    function<void()> task;
                    // The reason for putting the below code
                    // here is to unlock the queue before
                    // executing the task so that other
                    // threads can perform enqueue tasks
                    {
                        // Locking the queue so that data
                        // can be shared safely
                        unique_lock<mutex> lock(
                            queue_mutex_);

                        // Waiting until there is a task to
                        // execute or the pool is stopped
                        cv_.wait(lock, [this] {
                            return !tasks_.empty() || stop_;
                        });

                        // exit the thread in case the pool
                        // is stopped and there are no tasks
                        if (stop_ && tasks_.empty()) {
                            return;
                        }

                        // Get the next task from the queue
                        task = move(tasks_.front());
                        tasks_.pop();
                    }

                    task();
                }
            });
        }
    }

    // Destructor to stop the thread pool
    ~ThreadPool()
    {
        {
            // Lock the queue to update the stop flag safely
            unique_lock<mutex> lock(queue_mutex_);
            stop_ = true;
        }

        // Notify all threads
        cv_.notify_all();

        // Joining all worker threads to ensure they have
        // completed their tasks
        for (auto& thread : threads_) {
            thread.join();
        }
    }

    // Enqueue task for execution by the thread pool
    void enqueue(function<void()> task)
    {
        {
            unique_lock<std::mutex> lock(queue_mutex_);
            tasks_.emplace(move(task));
        }
        cv_.notify_one();
    }

private:
    // Vector to store worker threads
    vector<thread> threads_;

    // Queue of tasks
    queue<function<void()> > tasks_;

    // Mutex to synchronize access to shared data
    mutex queue_mutex_;

    // Condition variable to signal changes in the state of
    // the tasks queue
    condition_variable cv_;

    // Flag to indicate whether the thread pool should stop
    // or not
    bool stop_ = false;
};

int main()
{
    // Create a thread pool with 4 threads
    ThreadPool pool(4);

    // Enqueue tasks for execution
    for (int i = 0; i < 5; ++i) {
        pool.enqueue([i] {
            cout << "Task " << i << " is running on thread "
                 << this_thread::get_id() << endl;
            // Simulate some work
            this_thread::sleep_for(
                chrono::milliseconds(100));
        });
    }

    return 0;
}
```

# C++ : Iterator Invalidation

- https://www.geeksforgeeks.org/cpp/iterator-invalidation-cpp/
- https://lightcone.medium.com/iterator-invalidation-in-modern-c-ca0f3c161c5f
- https://learnmoderncpp.com/2024/09/04/understanding-iterator-invalidation/
- https://www.tutorialspoint.com/iterator-invalidation-in-cplusplus
- https://dotnettutorials.net/lesson/iterator-invalidation-in-cpp/
- https://intellipaat.com/blog/iterator-invalidation-cpp/
- https://labex.io/tutorials/cpp-how-to-resolve-iterator-lifetime-issues-419975

> When the container to which an Iterator points changes shape internally, i.e. when elements are moved from one position to another, and the initial iterator still points to the old invalid location, then it is called Iterator invalidation.

```c++
#include <bits/stdc++.h>
using namespace std;

int main()
{
    vector<int> v = { 1, 5, 10, 15, 20 };

    // Changing vector while iterating over it
    // (This causes iterator invalidation)
    for (auto it = v.begin(); it != v.end(); it++)
        if ((*it) == 5)
            v.push_back(-1);

    for (auto it = v.begin(); it != v.end(); it++)
        cout << (*it) << " ";
    return 0;
}
```

# C++ : STL Algorithms

- https://en.cppreference.com/w/cpp/algorithm.html
- https://www.geeksforgeeks.org/cpp/c-magicians-stl-algorithms/
- https://cplusplus.com/reference/algorithm/
- https://www.w3schools.com/cpp/cpp_ref_algorithm.asp
- https://www.quantstart.com/articles/C-Standard-Template-Library-Part-III-Algorithms/
- https://www.sandordargo.com/blog/2019/01/30/stl-algos-intro
- https://medium.com/@shrutipokale2016/c-stl-algorithms-one-shot-81ecede7657d
- https://www.geeksforgeeks.org/cpp/algorithms-library-c-stl/

> 1. Searching Algorithms
> 2. Sorting and Rearranging Algorithms
> 3. Manipulation Algorithms
> 4. Counting and Comparing Algorithms

# C++ : STL Iterators

- https://www.geeksforgeeks.org/cpp/iterators-c-stl/
- https://www.w3schools.com/cpp/cpp_iterators.asp
- https://en.cppreference.com/w/cpp/iterator.html
- https://www.programiz.com/cpp-programming/iterators
- https://cplusplus.com/reference/iterator/


> - Input
> - Output
> - Forward
> - Bidirectional
> - Random Access

![Image!](https://www.programiz.com/sites/tutorial2program/files/vector-iterator.png "Image")

```c++
#include <iostream>
#include<vector>
using namespace std;

int main() {
    vector <string> languages = {"Python", "C++", "Java"};
    vector<string>::iterator itr;

    // iterate over all elements
    for (itr = languages.begin(); itr != languages.end(); itr++) {
        cout << *itr << ", ";
    }
    return 0;
}
```
