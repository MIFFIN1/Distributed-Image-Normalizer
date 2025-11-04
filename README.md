# Distributed Image Normalizer

[cite_start]A project for COMP 7172 demonstrating a Distributed Image Normalizer built in Python[cite: 3]. [cite_start]It uses a master-worker architecture [cite: 4] to parallelize image preprocessing, allowing for benchmarks against traditional, centralized methods.

## Architecture

[cite_start]This system is built using a **Master-Worker** pattern[cite: 4].



[Image of Master-Worker architecture diagram]


1.  **Master Node**: The master is responsible for:
    * Discovering all images in the dataset.
    * [cite_start]Partitioning the list of images into smaller batches[cite: 13].
    * Assigning these batches (tasks) to available worker nodes.
    * [cite_start]Monitoring workers for failures[cite: 28].
    * [cite_start]Re-assigning tasks from failed workers[cite: 28].
    * [cite_start]Collecting and aggregating the normalized results into a final dataset[cite: 14].

2.  **Worker Nodes**: The workers are simple and responsible for:
    * Accepting a task (a list of image filepaths) from the master.
    * [cite_start]For each image, loading it and applying min-max pixel scaling[cite: 14].
    * Returning the normalized data (or success status) to the master.

## Project Goals

The primary goal is to **demonstrate and quantify the performance benefits of distributed computing** for a common ML preprocessing task. This is achieved by:

* [cite_start]**Scalability**: Proving that the total processing time decreases as the number of worker nodes increases[cite: 29].
* [cite_start]**Efficiency**: Distributing the load to avoid bottlenecks on a single machine[cite: 5].
* [cite_start]**Fault Tolerance**: Ensuring the system can complete its job even if a worker node fails during processing[cite: 28].

## Technology Stack

* **Language**: Python 3
* **Communication**: gRPC (A high-performance RPC framework)
* **Image Processing**: OpenCV-Python & NumPy
* [cite_start]**Testing**: Virtual Machines (VMs) or Docker containers to simulate multiple nodes[cite: 43].

## Getting Started

### Prerequisites

* Python 3.8+
* `pip` and `venv`

### Installation & Setup

1.  **Clone the repository:**
    ```bash
    git clone [https://github.com/](https://github.com/)[YOUR_USERNAME]/[YOUR_REPO_NAME].git
    cd [YOUR_REPO_NAME]
    ```

2.  **Create and activate a virtual environment:**
    ```bash
    python3 -m venv venv
    source venv/bin/activate
    ```

3.  **Install dependencies:**
    ```bash
    pip install -r requirements.txt
    ```

    *A sample `requirements.txt` might contain:*
    ```text
    grpcio
    grpcio-tools
    numpy
    opencv-python-headless
    ```

### How to Run

You will need multiple terminal windows to run the master and the workers.

1.  **Run the Master:**
    The master node will start and wait for workers to connect.
    ```bash
    python master.py --dataset_path /path/to/your/images
    ```

2.  **Run one or more Workers:**
    Open a new terminal for *each* worker you want to start.
    ```bash
    # Terminal 2
    python worker.py

    # Terminal 3
    python worker.py
    ```

## Benchmarking

[cite_start]To prove the system's scalability[cite: 29], you can run a series of tests:

1.  **Centralized (Baseline):** Run the system with only a `centralized_run.py` script that processes all images in a single thread.
2.  **Distributed (1 Worker):** Run the master and *one* worker. This includes network overhead and should be slower than the baseline.
3.  **Distributed (N Workers):** Run the master with 2, 4, 8, etc., workers.

Record the total time taken for each scenario. You should see the processing time drop significantly as `N` increases, proving the value of the distributed approach.

| Mode | # of Workers | Dataset Size | Time (seconds) |
| :--- | :--- | :--- | :--- |
| Centralized | 1 (N/A) | 1000 images | TBD |
| Distributed | 1 | 1000 images | TBD |
| Distributed | 2 | 1000 images | TBD |
| Distributed | 4 | 1000 images | TBD |

## Future Enhancements

* **Web Dashboard**: A simple web UI (e.g., using Flask or Streamlit) to upload a dataset, start the normalization job, and visualize the workers and progress in real-time.
* **Dynamic Scaling**: Automatically start or stop worker VMs based on queue size.
* **Advanced Aggregation**: Implement more complex aggregation logic beyond just collecting results.