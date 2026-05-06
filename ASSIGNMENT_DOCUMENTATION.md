# Assignment 3 - Complete Documentation

**Student Name**: Taif Saeed Alotaibi  
**Student ID**: 445052186  
**Date Submitted**: May 7, 2026

---

## 🎥 VIDEO DEMONSTRATION LINK (REQUIRED)

> **⚠️ IMPORTANT: This section is REQUIRED for grading!**
> 
> Upload your 3-5 minute video to your **PERSONAL Gmail Google Drive** (NOT university email).
> Set sharing to "Anyone with the link can view".
> Test the link in incognito/private mode before submitting.

**Video Link**: https://drive.google.com/file/d/1Bf-HZWeVpIB2WtRSTlf8bjNYqu3Ky4j_/view?usp=sharing

**Video filename**: `445052186_Assignment3_Synchronization.mp4`

**Verification**:
- [ c] Link is accessible (tested in incognito mode)
- [c ] Video is 3-5 minutes long
- [ c] Video shows code walkthrough and commits
- [ c] Video has clear audio
- [ c] Uploaded to PERSONAL Gmail (not @std.psau.edu.sa)

---

## Part 1: Development Log (1 mark)

### Entry 1 - May 3, 2026, 7:30 PM
**What I implemented**:  
I forked the assignment repository from GitHub and renamed it to my own repository. I also cloned it into Visual Studio Code.

**Challenges encountered**:  
I needed to make sure the repository was correctly linked to my GitHub account.

**How I solved it**:  
I followed the instructions to fork and clone the repository, then verified that it appeared in my GitHub account.

**Testing approach**:  
I opened the project in VS Code and confirmed that all files were accessible and ready for modification.

**Time spent**:  
30 minutes

---

### Entry 2 - May 5, 2026, 4:45 PM
**What I implemented**:  
I set my student ID in the main file and made my first commit.

**Challenges encountered**:  
Ensuring the correct student ID is displayed in the output.

**How I solved it**:  
I updated the `studentID` variable and verified it in the program output.

**Testing approach**:  
Ran the program and checked the header section.

**Time spent**:  
20 minutes

---

### Entry 3 - May 5, 2026, 5:30 PM
**What I implemented**:  
I added synchronization imports and declared locks and a semaphore in the `SharedResources` class.

**Challenges encountered**:  
Understanding which resources needed synchronization.

**How I solved it**:  
I identified shared variables and applied appropriate synchronization tools.

**Testing approach**:  
Ensured code compiles without errors.

**Time spent**:  
40 minutes

---

### Entry 4 - May 5, 2026, 6:15 PM
**What I implemented**:  
I implemented locking for shared counters and execution log.

**Challenges encountered**:  
Avoiding race conditions and ensuring proper lock release.

**How I solved it**:  
Used `ReentrantLock` with try-finally blocks.

**Testing approach**:  
Ran the program multiple times to check consistent values.

**Time spent**:  
50 minutes

---

### Entry 5 - May 5, 2026, 7:30 PM
**What I implemented**:  
I added semaphore control to the `run()` and `runToCompletion()` methods.

**Challenges encountered**:  
Handling InterruptedException and placing acquire/release correctly.

**How I solved it**:  
Used try-catch-finally structure and ensured release is always called.

**Testing approach**:  
Observed execution flow and confirmed no overlapping CPU execution.

**Time spent**:  
45 minutes
### Entry 6 - May 6, 2026, 1:00 AM
**What I implemented**:  
I recorded the video demonstration explaining the synchronization mechanisms used in the code, including locks and semaphore.

**Challenges encountered**:  
Ensuring that all required parts were clearly explained within the time limit.

**How I solved it**:  
I followed the assignment instructions and explained the code step by step while running the program.

**Testing approach**:  
I verified that the program runs correctly and produces consistent results before recording.

**Time spent**:  
1 hour

## Part 2: Technical Questions (1 mark)

### Question 1: Race Conditions
**Your Answer**:

The first race condition occurs in the shared counter variables such as `contextSwitchCount`. Multiple threads may execute `contextSwitchCount++` at the same time, and since this operation is not atomic, it can lead to lost updates and incorrect values.

The second race condition occurs in the `executionLog` (ArrayList). Since ArrayList is not thread-safe, multiple threads adding elements at the same time may cause inconsistent data or even runtime exceptions like `ConcurrentModificationException`.

To fix these issues, I used `ReentrantLock` to protect both the counters and the execution log, ensuring that only one thread accesses them at a time.

---

### Question 2: Locks vs Semaphores
**Your Answer**:

A `ReentrantLock` is used to provide mutual exclusion, meaning only one thread can access a critical section at a time. It is mainly used to protect shared variables.

A `Semaphore` controls access to a resource by allowing a limited number of threads to access it simultaneously.

In my implementation, I used `ReentrantLock` to protect shared counters and the execution log. I used a binary `Semaphore` (with one permit) to control CPU access, ensuring that only one process executes at a time.

---

### Question 3: Deadlock Prevention
**Your Answer**:

Deadlock is a situation where multiple threads are waiting for each other to release resources, causing them to be stuck forever.

One prevention technique is using `try-finally` blocks to ensure that locks are always released. Another technique is avoiding nested locks or inconsistent lock ordering.

In my code, I used `try-finally` to guarantee that both locks and semaphores are always released, which prevents deadlock.

---

### Question 4: Lock Granularity Design Decision
**Your Answer**:

I used one single lock (coarse-grained locking) to protect all three counters. I chose this approach because it simplifies the implementation and reduces the risk of deadlocks.

The trade-off is that coarse-grained locking reduces concurrency because only one thread can update any counter at a time. Fine-grained locking would allow more parallel updates but increases complexity.

Although the counters are independent, fine-grained locking provides better concurrency in theory. However, I preferred coarse-grained locking because it is safer and easier to manage for this assignment.

---

## Part 3: Synchronization Analysis (1 mark)

### Critical Section #1: Counter Variables

**Which variables**:  
contextSwitchCount, completedProcessCount, totalWaitingTime  

**Why they need protection**:  
They are shared between multiple threads and updated concurrently, which can lead to race conditions.  

**Synchronization mechanism used**:  
ReentrantLock  

**Code snippet**:
```]java
counterLock.lock();
try {
    contextSwitchCount++;
} finally {
    counterLock.unlock();
}
```
**Justification**: 
Locking ensures mutual exclusion so that only one thread updates the counters at a time, preventing incorrect values.
---

### Critical Section #2: Execution Log

**What resource**: 
executionLog (ArrayList)
**Why it needs protection**: 
Because multiple threads may add elements at the same time, which can corrupt the list or cause exceptions.

**Synchronization mechanism used**: 
ReentrantLock
**Code snippet**:
```java
logLock.lock();
try {
    executionLog.add(message);
} finally {
    logLock.unlock();
}
```


**Justification**: 
ArrayList is not thread-safe, so using a lock ensures safe access and modification.
---

### Critical Section #3: CPU Semaphore

**Purpose of semaphore**:
To control access to the CPU and ensure that only one process runs at a time. 

**Number of permits and why**: 
1 (binary semaphore), because only one thread should execute in the CPU at a time.

**Where implemented**: 
Inside run() and runToCompletion() methods.
**Code snippet**:
```java
SharedResources.cpuSemaphore.acquire();
try {
    // process execution
} finally {
    SharedResources.cpuSemaphore.release();
}
```

**Effect on program behavior**: 
It ensures that processes execute sequentially without overlap, maintaining correct scheduling behavior.
---

## Part 4: Testing and Verification (2 marks)

### Test 1: Consistency Check
**What I tested**: Running program multiple times to verify consistent results  

**Testing procedure**:
java SchedulerSimulationSync
java SchedulerSimulationSync
java SchedulerSimulationSync
java SchedulerSimulationSync
java SchedulerSimulationSync


**Results**: 
All runs produced consistent values. For example, the total completed processes remained 15, and context switches were consistent with the scheduling behavior.
from my output:

═══ Synchronization Statistics ═══
═══ Synchronization Statistics ═══
Total Context Switches: 29
Total Completed Processes: 15
Total Waiting Time: 797277ms
Average Waiting Time: 53151ms

═══ Process Summary Table ═══
Process    Priority     Burst Time   Waiting Time
────────────────────────────────────────────────
P1         4            3346         37          
P2         5            7852         54376       
P3         1            4279         58291       
P4         2            2586         11598       
P5         1            5025         58671       
P6         4            4036         59703       
P7         3            4537         59765       
P8         5            5331         60306       
P9         5            3412         30539       
P10        5            5905         61649       
P11        4            5784         63559       
P12        2            8939         73539       
P13        2            9618         74482       
P14        4            8765         76109       
P15        2            3540         54653       

═══ Execution Log Summary ═══
Total log entries: 58


**Why synchronization is necessary**: 
Without synchronization, race conditions could occur when multiple threads update shared variables like contextSwitchCount, completedProcessCount, and totalWaitingTime. This could lead to incorrect or inconsistent results. Also, executionLog could become corrupted or throw exceptions because it is not thread-safe.

**Conclusion**: 
Synchronization ensures stable, correct, and repeatable results across multiple runs.

---

### Test 2: Exception Testing
**What I tested**: Checking for ConcurrentModificationException

**Testing procedure**: 
Ran the program multiple times while monitoring the execution log behavior.

**Results**: 
No exceptions occurred after applying locks.

**What this proves**: 
The execution log is now thread-safe and protected from concurrent modification issues.

---

### Test 3: Correctness Verification
**What I tested**: Verifying correct final values (total burst time, context switches, etc.)

**Expected values**: 
Total completed processes = 15
**Actual values**: 
Total completed processes = 15
**Analysis**: 
The actual output matches the expected values, which confirms that synchronization is working correctly and no updates were lost.
---

### Test 4: Running the program with different randomly generated values (different burst times and priorities)

**Purpose**: 
To verify that synchronization works under different conditions
**Results**: 
The program executed correctly in all scenarios and produced consistent outputs
**What I learned**: 
Synchronization ensures that the program behaves correctly regardless of input variations
---

## Part 5: Reflection and Learning

### What I learned about synchronization:

I learned that synchronization is essential in multithreaded systems to prevent race conditions. I understood how shared variables can cause incorrect results if accessed by multiple threads without control. Using ReentrantLock ensures mutual exclusion, while Semaphore controls access to shared resources. I also learned the importance of using try-finally blocks to release locks safely. Debugging synchronization issues can be challenging, but proper implementation improves system reliability. Overall, synchronization ensures correctness, consistency, and stability in concurrent programs.
---

### Real-world applications:

Give TWO examples where synchronization is critical:

**Example 1**: 
Banking systems where multiple users access and update the same account
**Example 2**: 
Operating systems managing CPU scheduling and process execution
---

### How I would explain synchronization to others:

Synchronization is like controlling access to a shared room. Only one person is allowed inside at a time to avoid conflicts. Locks and semaphores act like keys that control who can enter and when. Without synchronization, multiple threads may interfere with each other and cause incorrect results.

---

## Part 6: GitHub Repository Information

**Repository URL**: 
https://github.com/Taif-Saeed-1/OS-Assignment3-Taif-Alotaibi

**Number of commits**: 
11

**Commit messages**: 
1. Set student ID
2. Add synchronization imports
3. Add locks and semaphore
4. Protect context switch counter
5.Protect completed process counter
6.Protect waiting time calculation
7.Protect execution log
8.Add semaphore to run method
9.Add semaphore to runToCompletion
10.Complete assignment documentation
11.Revise REFLECTION.md with detailed insi

---

## Summary

**Total time spent on assignment**: 
Approximately 4 days (distributed work sessions)

**Key takeaways**: 
1. Importance of synchronization in multithreading
2. Difference between locks and semaphores
3.Preventing race conditions and deadlocks

**Most challenging aspect**: 

Understanding where to place semaphore correctly

**What I'm most proud of**: 
Successfully implementing synchronization and achieving consistent results
---

**End of Documentation**
