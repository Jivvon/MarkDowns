1. The process control block(PCB) of a process is a data structure that contains attributes and control information of a process. The information in the PCB is important, therefore the kernel stores and accesses the PCBs in a permanent, non-volatile memory.

> X

2. Message passing과 shared memory 방법 중 빠르게 동작하는 것은 shared memory 방법이지만 동기화(synchronization)를 응용 프로그래머가 담당해야 하는 부담이 있다.

> O

3. 새로운 프로세스를 만들 때 가장 시간이 걸리는 작업은 프로세스의 유저 컨텍스트를 만드는 일이다. 새 프로세스의 데이터 영역과 스택 영역을 만들기 위해서 부모 프로세스의 데이터와 스택을 복사해서 만드는데, 이것이 시간이 걸리는 작업이다. 코드 영역은 복사해서 새로 만들지 않고 부모의 코드 영역을 같이 공유해서 쓴다. 코드는 공유 가능한 영역이기 때문에 복사하지 않음으로써 시간을 줄이고 메인 메모리 공간을 절약하기 위해서 이다.

> O

4. System calls provide the interface between a process and the operating system, whereas the command interpreter is the interface between the user and the operating system.

> O

5. 
   프로세스가 다른 프로세스를 생성하면 서로 부모-자식 프로세스 관계가 성립한다. 부모-자식 관계가 되면 자식 프로세스의 user context는 부모 프로세스의 user context에 연결된다. 따라서 자식 프로세스들이 2개 이상일 경우, 부모 프로세스 밑에 자식 프로세스 user context들이 연결되는 tree 구조를 이룬다.

> X

6. 모든 프로세스는 실행될 때 user mode와 system mode를 번갈아 가며 반복 실행한다.

> O

7. 프로세스 코드의 마지막 문장을 실행하여 성공적으로 실행이 마쳐진 프로세스는 실행(running) 상태에서 부모 프로세스에게 death-of-child 시그널을 보내고 그 프로세스가 가진 모든 자원을 반납한 후 실행 상태에서 빠져 나오면서 컴퓨터에서 사라진다.

> X

8. User Context includes code (instructions to be executed), data (global variable of the process), user stack. System Context includes the kernel stack and a Process Control Block (PCB). Kernel stack is used to store the information to invoke kernel functions.

> O

9. Time sharing is a logical extension of multiprogramming. Multiple jobs are executed by switching CPU time slots(time quantum) among them. Therefore the average number of jobs finished in a given time period by time sharing system is bigger than that of multiprogramming system.

> X

10. When a suspended blocked process is awaken by the completion of an input request, the state of the process is changed to ready state. Then this process can take the CPU and run when it is scheduled.

> X

11. Multiprogramming에서는 프로세스가 CPU를 잡으면 I/O와 같은 blocking system call이 나올 때까지 계속 CPU를 사용하지만, time sharing은 blocking system call이 나오거나 또는 time slice(quantum)가 경과하면 다른 프로세스에게 CPU를 넘긴다. 따라서 Multiprogramming 은 background(batch) 모드 처리에 적당하고, time sharing 은 foreground(interactive) 모드 처리에 적당한 방법이다.

> O

12. 모든 프로세스는 버추얼 어드레스 스페이스(virtual address space)를 하나씩 가진다. 버추얼 어드레스 스페이스는 보조기억장치에 만들어지는데, 32비트 컴퓨터의 경우 버추얼 어드레스 스페이스 크기가 4 Giga byte이므로 보조기억장치 4기가 용량을 차지하게 된다.

> X