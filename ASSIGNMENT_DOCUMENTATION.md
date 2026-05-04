# Assignment 3 - Complete Documentation

**Student Name**: [Ghala Muslim AL-Dossari]  
**Student ID**: [445052023]  
**Date Submitted**: [May 4, 2026]
---

## 🎥 VIDEO DEMONSTRATION LINK (REQUIRED)

> **⚠️ IMPORTANT: This section is REQUIRED for grading!**
> 
> Upload your 3-5 minute video to your **PERSONAL Gmail Google Drive** (NOT university email).
> Set sharing to "Anyone with the link can view".
> Test the link in incognito/private mode before submitting.

**Video Link**: [Paste your personal Gmail Google Drive link here]

**Video filename**: 445052023_Assignment3_Synchronization.mp4`

**Verification**:
- [ ] Link is accessible (tested in incognito mode)
- [ ] Video is 3-5 minutes long
- [ ] Video shows code walkthrough and commits
- [ ] Video has clear audio
- [ ] Uploaded to PERSONAL Gmail (not @std.psau.edu.sa)

---

## Part 1: Development Log (1 mark)

Document your development process with **minimum 3 entries** showing progression:

### Entry 1 - [May 3, 2026 - 7:00 PM]
**What I implemented**: 
I set up the project, updated my student ID, and studied the provided code to identify potential race conditions.
**Challenges encountered**: 
Understanding where race conditions occur in multithreaded environments.
**How I solved it**: 
I reviewed the course textbook and analyzed shared variables in the code.
**Testing approach**: 
Ran the program without synchronization to observe behavior.
**Time spent**: 
1.5 hours
---

### Entry 2 - [May 4, 2026 - 6:30 PM]
**What I implemented**: 
Added ReentrantLock to protect shared counters and execution log.
**Challenges encountered**: 
Ensuring locks are correctly placed without causing deadlock.
**How I solved it**: 
Used try-finally blocks to guarantee unlocking.
**Testing approach**: 
Tested multiple runs to ensure consistent counter values.
**Time spent**: 
2 hours
---

### Entry 3 - [May 5, 2026 - 5:00 PM]
**What I implemented**: 
Implemented Semaphore to control CPU access.
**Challenges encountered**: 
Understanding where to acquire and release the semaphore.
**How I solved it**: 
Placed acquire before execution and release in finally block.
**Testing approach**: 
Verified that only one process executes at a time.
**Time spent**: 
1.5 hours
---

### Entry 4 - [May 5, 2026 - 8:00 PM]
**What I implemented**: 
Tested the program thoroughly and verified synchronization behavior.
**Challenges encountered**: 
Ensuring no ConcurrentModificationException occurs.
**How I solved it**: 
Protected ArrayList with lock.
**Testing approach**: 
Ran program 5 times and compared outputs.
**Time spent**: 
1 hour
---

### Entry 5 - [May 6, 2026 - 4:00 PM]
**What I implemented**: 
Completed documentation and reviewed all code.
**Challenges encountered**: 
Writing clear explanations for synchronization concepts.
**How I solved it**: 
Referenced textbook definitions.
**Testing approach**: 
Final run and validation.
**Time spent**: 
1 hour
---

## Part 2: Technical Questions (1 mark)

### Question 1: Race Conditions
**Q**: Identify and explain TWO race conditions in the original code. For each:
- What shared resource is affected?
- Why is concurrent access a problem?
- What incorrect behavior could occur?

**Your Answer**:

Two race conditions exist in the original code.
First, the variable contextSwitchCount is shared among multiple threads. If two threads increment it simultaneously, one update may be lost, resulting in incorrect counts.
Second, the executionLog ArrayList is not thread-safe. Multiple threads adding elements concurrently may cause inconsistent state or even a ConcurrentModificationException.
These issues occur because there is no synchronization mechanism to control access to shared resources.
Without protection, the program may produce incorrect statistics and unpredictable results.
---

### Question 2: Locks vs Semaphores
**Q**: Explain the difference between ReentrantLock and Semaphore. Where did you use each in your code and why?

**Your Answer**:

ReentrantLock is used to provide mutual exclusion, allowing only one thread to access a critical section at a time. In my code, I used it to protect shared counters and the execution log.
Semaphore, on the other hand, controls access to a resource by allowing a fixed number of threads. I used a binary semaphore (1 permit) to simulate CPU access, ensuring that only one process executes at a time.
Locks are ideal for protecting data, while semaphores are useful for resource management.
This combination ensures both data consistency and controlled execution.
---

### Question 3: Deadlock Prevention
**Q**: What is deadlock? Explain TWO prevention techniques and what you did to prevent deadlocks in your code.

**Your Answer**:

Deadlock is a situation where two or more threads wait indefinitely for each other to release resources.
One prevention technique is using try-finally blocks to ensure locks are always released.
Another technique is maintaining consistent lock acquisition order.
In my code, I used try-finally blocks to guarantee that locks and semaphores are released even if an exception occurs.
This prevents threads from being blocked forever.

---

### Question 4: Lock Granularity Design Decision 
**Q**: For Task 1 (protecting the three counters), explain your lock design choice:
- Did you use ONE lock for all three counters (coarse-grained) OR separate locks for each counter (fine-grained)?
- Explain WHY you made this choice
- What are the trade-offs between the two approaches?
- Given that the three counters are independent, which approach provides better concurrency and why?

**Your Answer**:

I used a single lock (coarse-grained locking) to protect all three counters.
This approach simplifies implementation and reduces the risk of deadlocks.
However, it reduces concurrency because only one thread can update any counter at a time.
Fine-grained locking would allow multiple threads to update different counters simultaneously, improving performance.
Since the counters are independent, fine-grained locking provides better concurrency.
However, for simplicity and correctness, I chose coarse-grained locking.
---

## Part 3: Synchronization Analysis (1 mark)

### Critical Section #1: Counter Variables

**Which variables**: 
contextSwitchCount, completedProcessCount, totalWaitingTime
**Why they need protection**: 
They are shared among multiple threads and updated concurrently.
**Synchronization mechanism used**: 
ReentrantLock
**Code snippet**:
```java
// Paste your implementation here

lock.lock();
try {
    contextSwitchCount++;
} finally {
    lock.unlock();
}
```

**Justification**: 

---Ensures atomic updates and prevents race conditions.

### Critical Section #2: Execution Log

**What resource**: 
ArrayList executionLog
**Why it needs protection**: 
ArrayList is not thread-safe.
**Synchronization mechanism used**: 

ReentrantLock
**Code snippet**:
```java
// Paste your implementation here
lock.lock();
try {
    executionLog.add(message);
} finally {
    lock.unlock();
}



```

**Justification**: 
Prevents concurrent modification errors.
---

### Critical Section #3: CPU Semaphore

**Purpose of semaphore**: 
Control access to CPU
**Number of permits and why**: 
1 → simulate single CPU
**Where implemented**: 
Inside run() method
**Code snippet**:
```java
// Paste your implementation here
SharedResources.cpuSemaphore.acquire();
try {
    // execution
} finally {
    SharedResources.cpuSemaphore.release();
}

```

**Effect on program behavior**: 
Ensures only one process executes at a time.
---

## Part 4: Testing and Verification (2 marks)

### Test 1: Consistency Check
**What I tested**: Running program multiple times to verify consistent results

**Testing procedure**: 
```bash
# Commands used (run the program at least 5 times)
```
java SchedulerSimulationSync
java SchedulerSimulationSync
java SchedulerSimulationSync
java SchedulerSimulationSync
java SchedulerSimulationSync
**Results**: 
Consistent output with correct statistics.
12                                                               
   Time Quantum:  4000ms                                                           
  Student ID:    445052023                                                        
   Sync Mode:     Mutex Locks & Semaphores                                         


  ➕ P1 (Priority: 5) added to ready queue │ Burst time: 4916ms
  ➕ P2 (Priority: 2) added to ready queue │ Burst time: 7935ms
  ➕ P3 (Priority: 5) added to ready queue │ Burst time: 3371ms
  ➕ P4 (Priority: 1) added to ready queue │ Burst time: 9043ms
  ➕ P5 (Priority: 3) added to ready queue │ Burst time: 9965ms
  ➕ P6 (Priority: 4) added to ready queue │ Burst time: 3142ms
  ➕ P7 (Priority: 2) added to ready queue │ Burst time: 6350ms
  ➕ P8 (Priority: 3) added to ready queue │ Burst time: 8954ms
  ➕ P9 (Priority: 5) added to ready queue │ Burst time: 4001ms
  ➕ P10 (Priority: 2) added to ready queue │ Burst time: 8932ms
  ➕ P11 (Priority: 5) added to ready queue │ Burst time: 6851ms
  ➕ P12 (Priority: 2) added to ready queue │ Burst time: 5293ms

**Why synchronization is necessary**: 
Without synchronization, shared variables may produce incorrect values due to race conditions.
**Conclusion**: 
Synchronization ensures correctness.
---

### Test 2: Exception Testing
**What I tested**: Checking for ConcurrentModificationException

**Testing procedure**: 

**Results**: 
No ConcurrentModificationException occurred.
**What this proves**: 
Execution log is properly synchronized.
---

### Test 3: Correctness Verification
**What I tested**: Verifying correct final values (total burst time, context switches, etc.)

**Expected values**: 
Total processes = 12
**Actual values**: 
 12                                                               
Time Quantum:  4000ms                                                           
 Student ID:    445052023                                                        
 Sync Mode:     Mutex Locks & Semaphores 
Total Completed Processes = 12

**Analysis**: 
Results match expectations.
---

### Test 4: Different Scenarios
**Scenario tested**: [e.g., different time quantum, more processes, etc.]
Different time quantum
**Purpose**: 
Test scheduling behavior
**Results**: 
Program behaved correctly
**What I learned**: 
Synchronization works under different conditions.
---

## Part 5: Reflection and Learning

### What I learned about synchronization:

I learned how race conditions affect shared data in multithreaded systems.
I understood the importance of synchronization mechanisms such as locks and semaphores.
I learned how to apply mutual exclusion to protect critical sections.
I also learned how semaphores control resource access.
Debugging synchronization issues was challenging but improved my understanding.
This assignment helped me connect theory with practical implementation.

---

### Real-world applications:

Give TWO examples where synchronization is critical:

**Example 1**: Banking systems (account balance updates)


**Example 2**: Operating system process scheduling

---

### How I would explain synchronization to others:

Synchronization is like allowing only one person to use a shared resource at a time.
Without it, multiple users may interfere and cause incorrect results.

---

## Part 6: GitHub Repository Information

**Repository URL**:  https://github.com/Ghala2026/OS-Assignment3-Ghala-AL-Dossari.git

**Number of commits**: 12

**Commit messages**: 
1. change my student ID to 445052023
2. change my student ID to 445052023
3. Add ReentrantLock for shared counters
4. adding Semaphore and import its package for synchronization
5. Add lock protection for execution log
6. shared variable name (completedProcessCount) name lock and in finally…
7. protecting shared variable (totalWaitingTime)using Reentlock
8. protecting executionlog
9. Implement semaphore acquire and release in process execution
10. Implement semaphore acquire and release in process execution
11. Apply synchronization to runToCompletion method
12. Apply synchronization to runToCompletion method

---

## Summary

**Total time spent on assignment**: 1 days

**Key takeaways**: 
1- Understanding race conditions and how they affect shared data in multithreaded systems.
2- Learning how to use ReentrantLock to ensure mutual exclusion in critical sections.
3- Applying semaphores to control access to shared resources like CPU.
**Most challenging aspect**: 
The most challenging part was correctly identifying all critical sections and ensuring proper synchronization without introducing deadlocks.
**What I'm most proud of**: 
I am most proud of successfully implementing synchronization mechanisms that ensure consistent and correct program behavior across multiple executions.
---

**End of Documentation**
