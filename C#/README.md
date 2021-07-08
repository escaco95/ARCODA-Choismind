<h3 align="center">About C# Study</h3>
<p align="center">
  C#에 관한 이야기.
</p>

- [멀티스레딩](#멀티스레딩)
  - [System.Threading.Thread](#SystemThreadingThread)
  - [System.Threading.Tasks.Task](#SystemThreadingTasksTask)
- [멀티스레딩 노하우](#멀티스레딩-노하우)
  - [Thread vs Task](#Thread-vs-Task)

# 멀티스레딩

C#이 멀티스레드 환경을 구현하기 위해 제공하는 기능들과, 기능의 특징을 정리합니다.

# System.Threading.Thread

[레퍼런스](https://docs.microsoft.com/ko-kr/dotnet/api/system.threading.thread?view=net-5.0)에 따르면, `System.Threading.Thread`는 Start Sleep Yield 등의 기본적인 기능을 제공하며, 그 외에도 자잘한 저레벨 수준의 부가 기능을 제공하고 있다. 후술된 [Task](#SystemThreadingTasksTask)와 비교하면 low-level concept로 제공되고 있다는 결론에 도달했다.

당연한 말이지만, 많은 수의 스레드가 생성되었을때 자중하는 기능 따위가 있을 리 없고, 이로 인해 잦은 [Context Switching](#Context-Switching) 발생으로 인한 총체적 성능 저하 위험성을 내포하고 있다.

# System.Threading.Tasks.Task

[레퍼런스](https://docs.microsoft.com/ko-kr/dotnet/api/system.threading.tasks.task?view=net-5.0)에 따르면, `System.Threading.Tasks.Task`는 [System.Threading.ThreadPool](#SystemThreadingThreadPool)을 사용하여 보다 쾌적한 어플리케이션 실행 환경을 제공한다고 한다.

# 멀티스레딩 노하우

개발 중인 프로젝트, 개발할 프로젝트, 조언을 남겨야 할 때 고려할법한 노하우를 정리합니다.

# Thread vs Task

이 프로젝트에는(또는, 조언을 할 때) [Thread](#SystemThreadingThread)를 사용하는 것이 좋은가? 아니면 [Task](#SystemThreadingTasksTask)를 사용하는 게 좋은가?

**[System.Threading.Thread](#System.Threading.Thread)를 무지성으로 생성-실행할 경우:**
 - 10,000개의 스레드가 너도나도 서비스되기 위해 무수한 [Context Switching](#Context-Switching)을 일으키며 총체적 성능 저하가 발생

**[System.Threading.Tasks.Task](#SystemThreadingTasksTask)를 무지성으로 생성-실행할 경우:**
 - 10,000개의 태스크가 [System.Threading.ThreadPool](#SystemThreadingThreadPool)에 의해 큐로 관리되어, [ThreadPool.SetMaxThreads(...)](https://docs.microsoft.com/ko-kr/dotnet/api/system.threading.threadpool.setmaxthreads?view=net-5.0)로 설정된 만큼만 찔끔찔끔 실행된다. [Context Switching](#Context-Switching)으로 인한 부하가 최소로 발생하도록 적절한 값을 설정하여 위의 방법보다 속도를 높여나가는 게 핵심 노하우라고 할 수 있겠다.

**Conclusion:**

- [Thread](#SystemThreadingThread)
 
   > 개인적인 프로젝트와 조언. Abstraction보다 기본적인 기능을 직접 사용하는 스타일이거나, 충분한 이해를 바탕으로 고도의 Customization을 수행해야 할 때 사용
   
   > low-level concept으로 제공되는 모든 기능이 그렇지만, 휴먼 에러의 발생에 보다 더 관심을 기울일 것 
   
- [Task](#SystemThreadingTasksTask)
 
   > Flow와 사용법을 충분히 이해하고 있다면, 고도로 abstraction되어 있는 다양한 기능을 활용하여 깔끔한 코드의 작성이 가능할 것
   > 반대로 abstraction에 의해 발생할 수 있는 [Task](#SystemThreadingTasksTask)만의 issue에 대해서도 파악하고 있어야 한다
 
 
