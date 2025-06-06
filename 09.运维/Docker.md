# DOCKER

[TOC]

## 1. 解释 Docker 架构？

Docker 架构包含一个 Docker 引擎，它是一个客户端-服务器应用程序，具有三个主要组件：

1. 一个服务器，它是一种长时间运行的程序，称为守护进程（`docker` 命令）。
2. 一个 REST API，它指定程序可以用来与守护进程通信并指示其操作的接口。
3. 一个命令行界面 (CLI) 客户端（`docker` 命令）。
CLI 使用 Docker REST API 通过脚本或直接 CLI 命令来控制或与 Docker 守护进程交互。许多其他 Docker 应用程序使用底层的 API 和 CLI。

## 2. Dockerfile 的用途是什么？

Dockerfile 是一组必须传递给 Docker 的指令，以便它可以通过从指定的 Dockerfile 中读取这些指令来自动构建镜像。 Dockerfile 是一个文本文档，其中包含用户可以在命令行上调用以组装镜像的所有命令。 使用 `docker build` 用户可以创建一个自动构建，该构建按顺序执行多个命令行指令。

## 3. 区分虚拟机和 Docker 容器

| 虚拟机 | Docker 容器 |
| --- | --- |
| 虚拟机帮助开发人员在单个物理服务器的硬件上运行和托管多个操作系统。 | Docker 容器帮助开发人员在单个虚拟机或服务器上使用相同的操作系统部署多个应用程序。 |
| Hypervisor 为客户操作系统提供整个虚拟机。 | 容器为运行应用程序提供隔离的环境/用户空间。在容器内所做的任何更改都不会反映在主机或同一主机的其他容器上。 |
| 这些虚拟机形成了系统硬件层的抽象，这意味着主机上的每个虚拟机都像物理机一样工作。 | 容器形成了应用程序层的抽象，这意味着每个容器构成一个不同的应用程序。 |

## 4. 如何创建 Docker 容器？

你可以从任何你选择的特定 Docker 镜像创建 Docker 容器，并且可以使用下面给出的命令来实现相同的目的：
`docker run -t -i command name`
上面的命令将创建容器并为你启动它。为了检查 Docker 容器是否已创建以及它是否正在运行，你可以使用以下命令。此命令将列出所有 Docker 容器及其在 Docker 容器运行的主机上的状态。
`docker ps -a`

## 5. 如何停止和重启 Docker 容器？

以下命令可用于停止具有容器 ID 的某个 Docker 容器：
`docker stop CONTAINER_ID`
以下命令可用于重启具有容器 ID 的某个 Docker 容器：
`docker restart CONTAINER_ID`

## 6. Docker 容器的商业扩展应用可以有多大的体量？

像 Google、Twitter 这样的 Web 部署中的最佳示例以及像 Heroku、dotCloud 这样的平台提供商中的最佳示例都运行在 Docker 上，它可以从数十万到数百万个并行运行的容器范围内扩展，前提是运行所有这些无数容器托管你的应用程序的主机上的操作系统和内存不会耗尽。

## 7. 当 Docker 容器退出时，我的数据会丢失吗？

当你的任何 Docker 容器退出时，数据不会丢失，因为你的应用程序为了保存数据而写入磁盘的任何数据都将保留。 这将一直进行到容器被显式删除为止。 Docker 容器的文件系统即使在 Docker 容器停止后仍然存在。数据保留的前提是数据存储在卷（Volume）或绑定挂载（Bind Mount）中。如果数据仅存在于容器的可写层（未挂载卷），删除容器后数据会丢失。

## 8. 为什么我的服务需要 10 秒才能重新创建或停止？

`docker-compose stop` 将尝试通过发送 `SIGTERM` 消息来停止特定的 Docker 容器。 一旦此消息传递完成，它将等待默认的 10 秒超时时间，一旦超过超时时间，它会向容器发送 `SIGKILL` 消息——以便强制杀死它。 如果你实际上在等待超时时间，那么这意味着容器在收到 `SIGTERM` 信号/消息时没有关闭。
为了解决这个问题，你可以执行以下操作：

* 确保你在 docker 文件中使用 JSON 格式的 CMD 和 ENTRYPOINT。
* 使用 `["program", "argument1", "argument2"]` 而不是像这样以纯字符串形式发送——`"program argument1 argument2"`。
* 使用字符串形式会使 Docker 使用无法正确处理信号的 bash 来运行该进程。Compose 始终使用 JSON 格式。
* 如果可能，请修改你打算运行的应用程序，方法是为 `SIGTERM` 信号添加一个显式的信号处理程序。
* 此外，将 `stop_signal` 设置为应用程序可以理解并且知道如何处理的正确信号。

## 9. 如何在同一主机上运行 Compose 文件的多个副本？

Docker 的 compose 利用项目名称为所有项目的容器和资源创建唯一的标识符。 为了运行同一项目的多个副本，你需要使用 `–p` 命令行选项设置自定义项目名称，或者你可以为此目的使用 `COMPOSE_PROJECT_NAME` 环境变量。

## 10. up、run 和 start 有什么区别？

在任何给定的场景中，你总是希望你的 `docker-compose up`。 使用 UP 命令，你可以启动或重新启动 `docker-compose.yml` 文件中定义的所有服务。 在“附加”模式下（这也是默认模式），我们将能够看到所有容器的所有日志文件。 在“分离”模式下，它在启动所有容器后退出，这些容器继续在后台运行，前台不显示任何内容。
使用 `docker-compose run` 命令，我们将能够运行根据业务需求需要运行的一次性或临时任务。 这需要提供你想要运行的服务名称，并且基于此，它将仅启动正在运行的服务所依赖的那些服务的容器。 使用 run 命令，你可以运行测试或执行任何管理任务，例如删除/添加数据到数据卷容器。 它也非常类似于 `docker run –ti` 命令，该命令会打开一个到容器的交互式终端，并返回与容器中进程的退出状态相匹配的退出状态。
使用 `docker-compose start` 命令，你只能重新启动先前已创建并已停止的容器。 此命令本身从不创建任何新的 Docker 容器。

## 11. “Docker 化”有什么好处？

Docker 化企业环境可帮助团队利用 Docker 容器形成类似 CaaS（容器即服务）的服务平台。 它为团队提供了必要的敏捷性、可移植性，并让他们能够控制在自己的网络/环境中。

## 12. 每个主机可以运行多少个容器？

根据 Docker 将托管容器的环境，可以有环境支持的任意数量的容器。 应用程序大小和可用资源（如 CPU 和内存）将决定可以在环境中运行的容器数量。 尽管容器自己创建新的 CPU，但它们绝对可以提供有效利用资源的方法。 容器本身非常轻量级，并且只在它们正在运行的进程期间持续存在。

## 13. 是否可以通过 COPY/ADD 或卷包含特定代码？

这可以通过在你的 docker 文件中添加 COPY 或 ADD 指令来轻松实现。 如果你想将代码与任何 Docker 镜像一起移动，例如，将代码从开发环境发送到暂存环境，或从暂存环境发送到生产环境，这将非常有用。
话虽如此，你可能会遇到需要同时使用这两种方法的情况。你可以让镜像使用 COPY 包含代码，并在 Compose 文件中使用卷以在开发期间包含主机中的代码。卷会覆盖镜像的目录内容。

## 14. 云自动化会很快取代容器化吗？

Docker 容器日益普及，并且肯定会成为任何专业的持续集成/持续开发管道中不可或缺的一部分。 话虽如此，每个组织的所有关键利益相关者都有同等的责任来应对采用日常萌芽技术的风险和收益的挑战。 在我看来，Docker 在那些认识到容器化后果的组织中将非常有效。

## 15. 识别 Docker 容器状态的方法是什么？

我们可以通过运行命令 `docker ps –a` 来识别 Docker 容器的状态，该命令将依次列出主机上所有可用的 docker 容器及其相应的状态。 从那里我们可以轻松识别感兴趣的容器以相应地检查其状态。

## 16. ‘docker run’ 和 ‘docker create’ 有什么区别？

可以注意到的最重要的区别是，通过使用 ‘docker create’ 命令，我们可以在停止状态下创建一个 Docker 容器。 我们还可以为其提供一个 ID，以便以后使用。
这可以通过使用带有选项 `–cidfile FILE_NAME` 的命令 ‘docker run’ 来实现，如下所示：
`‘docker run –cidfile FILE_NAME’`

## 17. Docker 容器在任何给定时间点可能处于哪些不同的状态？

Docker 容器在任何给定时间点可能处于四种状态。这些状态如下所示：

* 运行中
* 已暂停
* 重启中
* 已退出

## 18. 你能从 Docker 中移除一个已暂停的容器吗？

不行，无法从 Docker 中移除仅处于暂停状态的容器。 容器必须处于停止状态才能从 Docker 容器中移除。

## 19. Docker 中的容器有没有可能自行重启？

不行，这是不可能的。 默认的 `–restart` 标志设置为从不自行重启。 如果你想调整此设置，可以尝试一下。 或者，在使用 `docker run` 命令时，可以使用某些 docker 定义的策略。
可用的策略如下：

1. **Off：** 在此策略下，如果容器停止或失败，它将不会重新启动。
2. **On-failure：** 在此策略下，仅当容器遇到与用户无关的故障时才会自行重新启动。
3. **Unless-stopped：** 使用此策略可确保仅当用户执行命令停止容器时，容器才能重新启动。
4. **Always：** 在这种类型的策略中，无论故障或停止如何，容器始终会重新启动。

这些策略可以这样使用：
`docker run -dit — restart [restart-policy-value] [container_name]`

## 20. 移除容器的首选方法是 ‘docker rm -f’ 还是先 ‘docker stop’ 再 ‘docker rm’？

从 Docker 中移除容器的最佳和首选方法是使用 ‘docker stop’，因为它将允许向其接收者发送 `SIG_HUP` 信号，从而为他们提供完成所有最终确定和清理任务所需的时间。 一旦此活动完成，我们就可以使用 Docker 中的 ‘docker rm’ 命令轻松移除容器，从而也更新 docker 注册表。

## 21. Docker 镜像和容器有什么区别？

Docker 容器是 Docker 镜像的运行时实例。
Docker 镜像没有状态，其状态永远不会改变，因为它只是一组文件，而 Docker 容器具有其执行状态。

## 22. 解释 Dockerfile 中的 ONBUILD 指令？

`ONBUILD` 指令向镜像添加一个触发指令，该指令在以后将该镜像用作另一个构建的基础时运行。 当下游 `Dockerfile` 的 `FROM` 指令引用该镜像时，将执行触发器。
`ONBUILD` 指令就像上游 `Dockerfile` 给下游 `Dockerfile` 的一个指令。
例如，你可以使用 `ONBUILD` 来运行一个脚本，该脚本在构建下游镜像之前，会根据下游提供的源代码构建和标记上游镜像。

## 23. 你能在 Windows 上本地运行 Docker 容器吗？

是的，你可以使用 Windows 容器在 Windows 上本地运行 Docker 容器。 但是，你需要 Windows Server 2016 或更高版本，或者像 Windows 10 这样的 Windows 客户端版本。 此外，你的 Windows 系统必须支持容器，并且必须启用该功能。

## 24. Docker 与虚拟机的区别是什么？

Docker 是一种基于容器的技术，它允许你在称为容器的隔离环境中打包和运行应用程序。 容器比虚拟机更轻量级，因为它们共享主机的操作系统内核。 这意味着它们启动更快，使用的资源更少，并且更易于移植。
另一方面，虚拟机 (VM) 是物理计算机的软件仿真。 每个 VM 都有自己的操作系统、内核和应用程序。 这使得它们比容器更重，但它也为它们提供了更强的隔离性。

## 25. 在 Docker 上运行有状态应用程序是个好习惯吗？Docker 最适合哪些场景？

通常不建议在 Docker 上运行有状态应用程序。 有状态应用程序将其数据存储到本地文件系统。 如果你需要将应用程序移动到另一台计算机，检索数据会变得很痛苦。
Docker 最适合无状态应用程序，这些应用程序不需要在会话之间存储任何数据。 一些适合 Docker 的场景包括：

* 微服务
* Web 应用程序
* 批处理作业
* 持续集成和持续交付 (CI/CD) 管道

## 26. 什么是孤立卷以及如何移除它？

孤立卷是任何容器都不再使用的命名卷。 由于卷永远不会自动移除（因为这样做可能会破坏数据），因此需要注意这些孤立卷，否则最终可能会耗尽磁盘空间。
你可以使用以下命令移除孤立卷：
`docker volume rm <volume_name>`
或者，要一次性移除所有孤立卷：
`docker volume prune`

## 27. 容器在底层是如何工作的？

容器通过利用 Linux 内核的几个特性（如命名空间和 cgroup）在底层工作。

* **命名空间** 为容器提供自己的隔离系统视图。 例如，每个容器都有自己的进程 ID (PID) 命名空间、网络命名空间和挂载命名空间。 这可以防止容器相互干扰或干扰主机系统。
* **Cgroup**（控制组）限制容器可以使用的资源量，例如 CPU、内存和磁盘 I/O。 这有助于确保一个容器不会独占主机系统的所有资源并导致其他容器出现性能问题。
Docker 引擎使用这些 Linux 内核特性来创建和管理容器。 当你启动一个容器时，Docker 引擎会为该容器创建一组新的命名空间和 cgroup。 然后，它在这些命名空间和 cgroup 中启动容器的进程。

## 28. Docker 如何在非 Linux 系统中运行容器？

Docker 最初是为 Linux 设计的，它利用 Linux 内核特性（如命名空间和 cgroup）来运行容器。 但是，Docker 现在也可以在 Windows 和 macOS 上运行。
在非 Linux 系统上，Docker 使用轻量级虚拟机 (VM) 来运行 Linux 内核。 然后，此 VM 用于托管 Docker 容器。

* **在 Windows 上：** Docker Desktop for Windows 使用 Hyper-V（适用于 Windows 专业版/企业版/教育版）或 WSL 2 (Windows Subsystem for Linux 2)（适用于 Windows 家庭版和专业版）来运行 Linux VM。
* **在 macOS 上：** Docker Desktop for Mac 使用 HyperKit（一种基于 Hypervisor.framework 构建的轻量级 macOS 虚拟化解决方案）来运行 Linux VM。
此方法允许 Docker 容器在非 Linux 系统上运行，就好像它们在 Linux 上本地运行一样，同时利用特定于平台的虚拟化技术以获得最佳性能。

## 29. 如何将 Docker 与多个环境一起使用？

将 Docker 与多个环境（例如开发、测试和生产）一起使用的常见方法是使用不同的 Docker Compose 文件或使用环境变量来自定义每个环境的容器行为。
以下是一些策略：

* **不同的 Docker Compose 文件：** 为每个环境（例如，`docker-compose.yml` 用于开发，`docker-compose.prod.yml` 用于生产）创建单独的 Docker Compose 文件。 这些文件可以定义特定于环境的配置，例如不同的端口映射、卷或服务副本。
* **环境变量：** 在 Dockerfile 或 Docker Compose 文件中使用环境变量来参数化配置。 然后，你可以为每个环境设置不同的环境变量值。
* **覆盖文件：** Docker Compose 允许你使用多个 Compose 文件，其中后续文件会覆盖或扩展先前文件中的配置。 你可以有一个基础 `docker-compose.yml` 文件和特定于环境的覆盖文件（例如，`docker-compose.override.yml` 用于本地开发）。
* **配置管理工具：** 对于更复杂的设置，请考虑使用 Ansible、Puppet 或 Chef 等配置管理工具来管理不同环境中的 Docker 配置。

## 30. 命名一些容器与虚拟机的局限性

尽管容器提供了许多优势，但与虚拟机 (VM) 相比，它们也有一些局限性：

* **共享内核安全性：** 容器共享主机操作系统的内核。 如果内核中存在漏洞，则可能会影响主机上的所有容器。 VM 提供更强的隔离，因为每个 VM 都有自己的内核。
* **操作系统兼容性：** Docker 容器通常运行 Linux 应用程序。 虽然 Windows 容器存在，但你不能在 Linux 主机上运行 Windows 容器，反之亦然（没有 VM）。 VM 可以运行任何操作系统，而与主机操作系统无关。
* **资源密集型应用程序：** 对于需要保证资源（例如专用 CPU 内核或大量 RAM）的非常资源密集型的应用程序，VM 可能更合适，因为它们提供更强的资源隔离。
* **完整的操作系统环境：** 某些应用程序可能需要完整的操作系统环境，包括特定的内核模块或系统级守护进程，这些在容器化环境中可能不容易获得或配置。 VM 为此类应用程序提供了一个完整的操作系统。
* **图形用户界面 (GUI)：** 虽然可以运行带有 GUI 的容器化应用程序，但这通常比在 VM 中运行它们更复杂。 VM 更适合需要直接 GUI 访问的应用程序。

## 31. 为什么 Docker Compose 在转到依赖顺序中的下一个服务之前不等待容器准备就绪？

Docker Compose 按照 `depends_on` 中定义的依赖顺序启动服务，但它只等待依赖的服务*启动*，而不等待它们*准备好*处理请求。
“准备就绪”的概念因应用程序而异。 例如，数据库可能正在运行，但尚未准备好接受连接，或者 Web 服务器可能正在启动但尚未侦听请求。
Docker Compose 没有内置机制来了解每个应用程序的特定准备就绪状态。
要解决此问题，你需要在应用程序级别或使用其他工具来实现运行状况检查和依赖项等待逻辑。 一些常见的方法包括：

* **应用程序级重试：** 在你的应用程序中实现重试逻辑，以便在依赖项不可用时重试连接。
* **包装器脚本：** 使用包装器脚本（例如 `wait-for-it.sh` 或 `dockerize`）在启动主应用程序进程之前等待依赖服务可用。
* **运行状况检查：** 在 Docker Compose 文件中定义运行状况检查，并使用 `condition: service_healthy` 和 `depends_on` 来确保服务在依赖项运行状况良好后启动。 但是，请注意，`service_healthy` 仅在 Docker Compose 版本 2.3 及更高版本中受支持。
