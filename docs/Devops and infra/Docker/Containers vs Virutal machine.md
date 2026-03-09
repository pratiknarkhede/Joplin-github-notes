<img decoding="async" width="1225" height="501" src="../../_resources/5db8dc927ca2657cd7f866b2d405105d" alt="Docker containerized and vm transparent bg" data-lazy-srcset="https://www.docker.com/wp-content/uploads/2021/11/docker-containerized-and-vm-transparent-bg.png 1225w, https://www.docker.com/wp-content/uploads/2021/11/docker-containerized-and-vm-transparent-bg-980x401.png 980w, https://www.docker.com/wp-content/uploads/2021/11/docker-containerized-and-vm-transparent-bg-480x196.png 480w" data-lazy-sizes="(min-width: 0px) and (max-width: 480px) 480px, (min-width: 481px) and (max-width: 980px) 980px, (min-width: 981px) 1225px, 100vw" data-lazy-src="https://www.docker.com/wp-content/uploads/2021/11/docker-containerized-and-vm-transparent-bg.png" data-ll-status="loaded" class="entered lazyloaded" sizes="(min-width: 0px) and (max-width: 480px) 480px, (min-width: 481px) and (max-width: 980px) 980px, (min-width: 981px) 1225px, 100vw" srcset="https://www.docker.com/wp-content/uploads/2021/11/docker-containerized-and-vm-transparent-bg.png 1225w, https://www.docker.com/wp-content/uploads/2021/11/docker-containerized-and-vm-transparent-bg-980x401.png 980w, https://www.docker.com/wp-content/uploads/2021/11/docker-containerized-and-vm-transparent-bg-480x196.png 480w" style="box-sizing: border-box; margin: 0px; padding: 0px; border: 0px; outline: 0px; font-size: 16px; text-size-adjust: 100%; vertical-align: baseline; background: transparent; max-width: 100%; height: auto; pointer-events: none; position: relative;">

###   
<br/>

* * *

###   
<br/>Virtual Machines (VMs)  
<br/>Virtual Machines provide an environment for running an entire operating system (OS) - the "guest" OS - on top of another OS - the "host" OS.

### This is facilitated by a piece of software called a hypervisor. (e.g., VMware, VirtualBox)

### The hypervisor manages the virtual machines and allocates physical resources (like CPU, memory) from the host to each VM.

  
**Example:  
**Imagine you're running a Windows computer (the host), but you need to test software on Ubuntu Linux.

You can use a hypervisor (like VMware or VirtualBox) to create a VM that runs Ubuntu. Inside this VM,

you can install and test your software as if you were using a real Ubuntu machine, even though it's running on a Windows host.

&nbsp;

Each VM is isolated, meaning it has its own virtual hardware: CPU, memory, network interface, etc.

However, this also means that each VM can be quite resource-intensive since it's running a full copy of an operating system on top of the application and its dependencies. 

&nbsp;

&nbsp;

* * *

**Containers:**

**Containers, on the other hand, provide a way to package an application along with its dependencies (libraries, binaries, configuration files, etc.) into a single lightweight executable.  
<br/>Unlike VMs, containers share the host OS's kernel and, where appropriate, the bins/libraries. This makes containers significantly more lightweight and faster to start than VMs.  
<br/>**Example of Container Usage:** Continuing from the previous example, instead of running a full VM for your Ubuntu-based application, you could create a Docker container. This container includes the application and its dependencies but uses the Linux kernel of the host system (if you're on Linux)**

**or a lightweight VM if you're on Windows or Mac (since Docker needs a Linux kernel).**

**You can easily run multiple containers on a single host without the overhead of multiple operating systems.**

&nbsp;  
<br/>

&nbsp;

&nbsp;

* * *

**Additional Explanation  
<br/>How using ubuntu image in docker and vm is different  
<br/>**

### Using Ubuntu in Docker:

When you use an Ubuntu image in Docker, you're not running a full-blown Ubuntu operating system in the same sense as you are with a VM.

Instead, the container uses the Linux kernel of the host system and *==packages only the necessary Ubuntu user space binaries and libraries needed to run your application==*. This is a crucial distinction.

- **Shared Kernel:** Containers on the same host share the host's kernel. There is no separate kernel for each container, reducing overhead.
- **Lightweight:** Since containers only package the app and its immediate dependencies (not the entire OS), they are much lighter and use less disk space.
- **Efficiency:** Without the need to boot an OS, containers can start almost instantly, making them much more efficient in terms of startup time and resource usage.

&nbsp;

&nbsp;

### Using Ubuntu on a VM:

Installing Ubuntu on a VM involves setting up a full Ubuntu operating system, including not just the user space binaries and libraries but also the kernel, system services, and various background processes. Each VM runs its own full instance of an operating system on top of the virtualized hardware provided by the hypervisor.

- **Full OS:** Each VM runs a complete instance of the operating system, including its own kernel.
- **Resource Intensive:** VMs are more resource-intensive, as they require more memory and CPU to emulate virtual hardware and run separate instances of the operating system.
- **Slower Startup:** VMs have longer startup times because they need to boot the entire operating system.

&nbsp;

### In Practice:

Let's say you have an application that requires Ubuntu to run.

- **With Docker:** You pull an Ubuntu image (which includes only the user space part of Ubuntu necessary for your application) and create a container where you install and run your application. This container relies on the host's kernel, and only the additional layers needed for your application are added on top of the base Ubuntu image.
    
- **With a VM:** You install Ubuntu as a guest OS on a hypervisor, which includes setting up the entire Ubuntu system (kernel, system services, etc.) before installing and running your application. Each VM is isolated with its own full-blown OS.
    

&nbsp;