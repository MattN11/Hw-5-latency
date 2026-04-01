## Q1: What is COW and Why Does It Improve Fork Latency?
Copy-on-Write (COW) for Process Forking

  Copy-on-Write is a smart trick that operating systems use to make forking processes way faster and more efficient. Here's the thing: when you fork a process the traditional way, the kernel copies every single page of memory from the parent process to the child. If you have a process with like 1GB of memory, it literally copies all of it, which takes forever. 
But here's where COW comes in and saves the day. With COW, when you fork, the kernel doesn't actually copy anything right away. Instead, it just creates a new page table for the child process, but both the parent and child point to the same physical pages in memory. The trick is that both processes are marked as read-only for those pages. 
So if either the parent or the child tries to write to one of these pages, BAM, you get a page fault. The kernel catches this fault and only then does it copy that specific page. So if the child process just calls exec() right after forking, it completely replaces its memory with a new program, so copying never even happens. 
This makes fork latency basically instant and independent of memory size. Plus, both processes are sharing the same physical pages for a while, which also saves memory overall.

