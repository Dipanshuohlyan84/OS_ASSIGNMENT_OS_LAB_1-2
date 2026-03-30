# OS_ASSIGNMENT_OS_LAB_1-2

 Operating Systems Lab Assignments
 Overview
This repository contains implementations of core Operating System algorithms developed as part of the OS Lab coursework.

It includes:

CPU Scheduling Algorithms (FCFS & SJF)
Banker’s Algorithm for Deadlock Avoidance
All implementations are written in Python with proper structure, outputs, and analysis.

🚀 Navigation
Assignment 1 | Folder
Assignment 2 | Folder
Features
Structure
Outputs




 Assignment 1: CPU Scheduling
Algorithms Implemented
First Come First Serve (FCFS)
Shortest Job First (SJF - Non Preemptive)
⚙️ Functionality
Process creation with PID, Arrival Time, Burst Time
Calculation of:
Completion Time (CT)
Turnaround Time (TAT)
Waiting Time (WT)
Gantt Chart visualization
Average WT & TAT calculation
Comparison between FCFS and SJF
📊 Key Insight
SJF performs better than FCFS in minimizing waiting time by prioritizing shorter processes.




📘 Assignment 2: Banker’s Algorithm
🔹 Concept
Banker’s Algorithm is used to avoid deadlock by ensuring the system remains in a safe state.

⚙️ Functionality
Input:
Allocation Matrix
Maximum Matrix
Available Resources
Calculation:
Need Matrix = Maximum - Allocation
Safety Algorithm:
Checks if system is SAFE or UNSAFE
Generates Safe Sequence
Step-by-step execution tracing
🔐 Key Insight
The system avoids deadlock by only allowing execution when resources satisfy:

Need ≤ Available

✨ Features
✔ Clean and modular Python implementation
✔ Proper matrix representation
✔ Step-by-step execution output
✔ Gantt Chart (Assignment 1)
✔ Safe Sequence detection (Assignment 2)
✔ Beginner-friendly code with comments
✔ Accurate calculations and logic







📁 Project Structure
OS-Lab/
│
├── Assignment-1/
│ ├── code.ipynb
│ └── Lab_Report-Assignment-1
├── Assignment-2/
│ ├── code.ipynb
│ └── Lab_Report-Assignment-2
│
└── README.md

📸 Outputs
🔹 Assignment 1
Input Process Table
FCFS Output Table
SJF Output Table
Gantt Charts
Average Time Comparison
🔹 Assignment 2
Allocation Matrix
Maximum Matrix
Need Matrix
Safe Sequence
System State (SAFE / UNSAFE)
📊 Sample Output Highlights
CPU Scheduling
FCFS Avg WT: Higher
SJF Avg WT: Lower

Banker’s Algorithm
System is SAFE
Safe Sequence: P1 -> P3 -> P4 -> P0 -> P2

🎯 Learning Outcomes
Understanding CPU scheduling techniques
Implementing optimization-based scheduling (SJF)
Learning deadlock avoidance strategies
Working with matrices and system states
Improving problem-solving and coding logic
🛠️ Technologies Used
Python 3.x
Jupyter Notebook
Standard Python Libraries
Command Line Interface
📌 Notes
All algorithms are implemented from scratch
Proper edge cases handled (idle CPU, unsafe state)
Outputs are verified for correctness
👨‍💻 Author
DIPANSHU OHLYAN
BCA (AI & Data Science) Roll No : 2401201062

















Task 1: Process Class + Input Handling
To do:
Create Process class (PID, AT, BT)
Take input for 4–5 processes
Store in list
Print table
1.1 Create Class
class Process:
    def __init__(self, pid, at, bt):
        self.pid = pid      # Process ID
        self.at = at        # Arrival Time
        self.bt = bt        # Burst Time
        self.ct = 0         # Completion Time
        self.tat = 0        # Turnaround Time
        self.wt = 0         # Waiting Time
1.2 Take Input
processes = []
n = int(input("Enter number of processes: "))
for i in range(n):
    print(f"\nEnter details for Process {i+1}")
    
    at = int(input("Arrival Time: "))
    bt = int(input("Burst Time: "))
    
    processes.append(Process(i+1, at, bt))
Enter details for Process 1

Enter details for Process 2

Enter details for Process 3

Enter details for Process 4

Enter details for Process 5
1.3 Display Table
print("\nPID\tAT\tBT")
for p in processes:
    print(f"P{p.pid}\t{p.at}\t{p.bt}")
PID	AT	BT
P1	0	7
P2	2	4
P3	4	1
P4	5	4
P5	6	2
Task 2: FCFS Scheduling
Process:
Sort by Arrival Time

Execute one by one

Handle CPU idle










2.1 Sort Processes & Calculate Times
# Sort processes by Arrival Time
processes.sort(key=lambda p: p.at)

time = 0

for p in processes:
    
    # If CPU is idle
    if time < p.at:
        time = p.at
    
    # Completion Time
    p.ct = time + p.bt
    
    # Turnaround Time
    p.tat = p.ct - p.at
    
    # Waiting Time
    p.wt = p.tat - p.bt
    
    # Update current time
    time = p.ct






    
2.2 Display Output
fcfs_processes = fcfs([Process(p.pid, p.at, p.bt) for p in processes])

print("\nFCFS Scheduling:")
print("PID\tAT\tBT\tCT\tTAT\tWT")

for p in processes:
    print(f"P{p.pid}\t{p.at}\t{p.bt}\t{p.ct}\t{p.tat}\t{p.wt}")
FCFS Scheduling:
PID	AT	BT	CT	TAT	WT
P1	0	7	7	7	0
P2	2	4	11	9	5
P3	4	1	12	8	7
P4	5	4	16	11	7
P5	6	2	18	12	10
Task 3: SJF (Non-Preemptive)
Logic:
Only pick from arrived processes

Choose minimum BT









3.1 Sort and Calculate Times
def sjf(processes):
    processes = sorted(processes, key=lambda x: x.at)
    ready_queue = []
    completed = []
    time = 0

    while processes or ready_queue:

        # Add all arrived processes to ready queue
        while processes and processes[0].at <= time:
            ready_queue.append(processes.pop(0))

        if ready_queue:
            # Pick process with smallest burst time
            ready_queue.sort(key=lambda x: x.bt)
            current = ready_queue.pop(0)

            # If CPU was idle before this process
            if time < current.at:
                time = current.at

            # Completion Time
            current.ct = time + current.bt

            # Turnaround Time
            current.tat = current.ct - current.at

            # Waiting Time
            current.wt = current.tat - current.bt

            # Update time
            time = current.ct

            completed.append(current)

        else:
            # No process available → CPU idle
            time += 1

    return completed





    
3.2 Display Output
sjf_processes = sjf([Process(p.pid, p.at, p.bt) for p in processes])

print("\nSJF Scheduling (Non-Preemptive):")
print("PID\tAT\tBT\tCT\tTAT\tWT")

for p in sjf_processes:
    print(f"P{p.pid}\t{p.at}\t{p.bt}\t{p.ct}\t{p.tat}\t{p.wt}")
SJF Scheduling (Non-Preemptive):
PID	AT	BT	CT	TAT	WT
P1	0	7	7	7	0
P3	4	1	8	4	3
P5	6	2	10	4	2
P2	2	4	14	12	8
P4	5	4	18	13	9
Task 4: Gantt Chart
4.1 Define Format
def gantt_chart(processes, title):
    print(f"\nGantt Chart ({title}):")

    time = 0
    print("0", end=" ")

    for p in processes:
        if time < p.at:
            time = p.at  # handle idle

        time += p.bt
        print(f"| P{p.pid} | {time}", end=" ")

    print()








    
4.2 Display Chart (FCFS & SJF)
gantt_chart(processes, "FCFS")
gantt_chart(sjf_processes, "SJF")
Gantt Chart (FCFS):
0 | P1 | 7 | P2 | 11 | P3 | 12 | P4 | 16 | P5 | 18 

Gantt Chart (SJF):
0 | P1 | 7 | P3 | 8 | P5 | 10 | P2 | 14 | P4 | 18 
Task 5: Performance Analysis
Calculate And Display Averages for:
Waiting Time
Turn Around Time










5.1 First Come First Serve (FCFS)
avg_wt = sum(p.wt for p in processes) / len(processes)
avg_tat = sum(p.tat for p in processes) / len(processes)

print(f"\nAverage WT (FCFS): {avg_wt}")
print(f"Average TAT (FCFS): {avg_tat}")
Average WT (FCFS): 5.8
Average TAT (FCFS): 9.4
5.2 Shortest Job First (SJF)
avg_wt_sjf = sum(p.wt for p in sjf_processes) / len(sjf_processes)
avg_tat_sjf = sum(p.tat for p in sjf_processes) / len(sjf_processes)

print(f"\nAverage Waiting Time (SJF): {avg_wt_sjf}")
print(f"Average Turnaround Time (SJF): {avg_tat_sjf}")
Average Waiting Time (SJF): 4.4
Average Turnaround Time (SJF): 8.0
