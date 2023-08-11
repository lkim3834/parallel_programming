# Part 1 : Producer-consumer Queues
- the memory thread loads the value, and sends it to the trig thread for computation. Then, the trig thread performs the computation, and sends the value back to the memory thread.

## 1.1  A synchronous producer-consumer queue (./syncQueue, CQueueSync.h) 
- implement queue in synchronous way using atomic. 
- every enqueue must wait for the corresponding dequeue.
`atomic<float> box;`
`atomic_bool flag;`

- use of busy-wait. (while) 

### Asynchronous Producer-Consumer Queue Implementation
### 1.2 Optimizing Programs for Asynchronous Producer-Consumer Queues
**File Paths:**
- Async Queue: `./asyncQueue`
- Header File: `CQueue.h`

**Queue Configuration:**
- Utilizes a circular buffer with size defined as `CQUEUE_SIZE`.
- **Enqueue**: Spin if queue is full (`head +1 == tail`).
- **Dequeue**: Spin if queue is empty (`head == tail`).

### 1.3 Optimizing Programs for Asynchronous Producer-Consumer Queues

**File Paths:**
- Optimized Queue: `./queue8`
- Main Code: `main3.cpp`

**Functionality:**
- Processes 8 items concurrently using 8 consecutive enqueues and dequeues.

### 1.4 Further Optimization of Programs and Queues

**File Paths:**
- Optimized Queue API: `./queue8API`
- Main Code: `main4.cpp`
- Enqueue and Dequeue Functions in Header: `8enq` and `8deq` (within `CQueue.h`)

**Features:**
- Optimizes the async-consumer queue by introducing an API that can enqueue and dequeue 8 items concurrently.
- Ensure the queue has the necessary elements for both operations:

    ```cpp
    // Check for elements to dequeue
    while(size() < 8 ){};
    
    // Check for space to enqueue
    while( CQUEUE_SIZE - size() <= 9){};
    ```

### 1.5 Efficient Usage of Asynchronous Queues

**Considerations:**
- Waiting Time
- Buffering Overhead

**Key Insights:**
- Synchronous queues, without optimization, can outperform asynchronous ones because they avoid buffer management operations, thereby reducing overhead.
- For optimized async producer-consumer queues, there's a limit to the number of consecutive items before performance drops.
- Doubling queue size might raise throughput, but also latency. Data might wait longer before processing.

### Conclusions from the Experiment

Over the course of the experiment, it became evident that:
- Synchronous queues, especially unoptimized ones, often excel in efficiency compared to asynchronous queues.
- This efficiency arises from eliminating buffer management operations, reducing overhead in asynchronous queues.
- For optimized async producer-consumer queues, there's a performance degradation threshold concerning consecutive items.
- Enlarging queue size might enhance throughput but can also escalate latency, causing data items to queue for longer before processing.

### Domain Relevance

Throughput and latency dynamics are relevant in both the **embedded systems** and **networking** domains. Balancing these two metrics is pivotal in both sectors. Examining the thresholds for consecutive items and queue size has fostered a deeper understanding of these domains' balancing methodologies.
