# Containers vs Virtual Machines

Both, Container and VM are **virtualization technologies**. What differentiates them is what level of virtualization they are working on.

A Machine could be represented as a stack of layers. The bottom is the **Hardware/infrastructure**, then the **Kernel/Host operational system** and then the **Applications**.

The VM acts in the second layer **(Kernel/Host OS)**. In other words, the VM creates a separate environment, where each sub-box has its entire kernel, and upon the kernel, the respective libraries and applications. You can imagine that this is a heavy setup because the `Kernel` is the interface that interacts with memory and CPU. So, pretty dense.

On the other hand, the container acts in the third layer **Application**. It's a lightweight virtualization mode because it doesn't need to create a new kernel, **it uses the host OS**. But the trick happens because in that container you are able to set up `namespaces` and `cgroups`, which give this component the impression that it is running in a separate/magic environment, but not.

An important thing to mention, is for example, when you set up a container, the first thing, in the `Dockerfile`, is to create the `FROM` image layer.
This image is not the whole OS itself, **even seems like it**. If you take a look at the size of this image, it's possible to see that it's pretty small. So, inside of it, there are just some libraries, and components that will be used by the application, and **not the whole OS**.

It's also impossible to use Docker on a Windows machine because Windows doesn't allow the creation of the `namespaces` and `cgroups` that I mentioned before, tools that are crucial in a Docker environment. For this reason, WSL exists, it's a way to emulate a Linux environment on Windows and then you can use Docker.

**Just to summarize, the container, shares things with the host OS, and the VM doesn't because it creates a WHOLE new environment.**

# Vertical and Horizontal Scaling

- Vertical: Grow the size of the application. For example more memory or CPU.
- Horizontal: Grow the number of servers. For example more machines in a cluster.
