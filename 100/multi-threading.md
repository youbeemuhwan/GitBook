# Multi-Threading

## <mark style="color:yellow;">병렬과 동시성의 차이점을 말해주세요.</mark>

병렬은 실제로 두개의 작업을 동시에 진행하는것을 말하고 동시성은 번갈아가면서 작업 하되 그것이 빠른속도로 이루어져 마치 동시에 진행하는것처럼 느끼게 작업하는것이 동시성이다.

싱글코어에서 멀티쓰레드를 사용하는것 - 동시성

멀티코어에서 멀티쓰레드를 사용하는것 - 병렬

## <mark style="color:yellow;">스레드와 프로세스의 차이를 말해주세요.</mark>

쓰레드는 하나의 작업 안에서  나눌수있는 여러 흐름의 단위 이고

프로세스는 하나의 작업 그 자체를 의미한다. 고로 쓰레드는 프로세스 안에 속해있다고 볼수있다.

ex. 하나의 프로세스 안에 여러개의 쓰레드&#x20;

## <mark style="color:yellow;">데몬 스레드는 무엇인가요?</mark>

메인 스레드를 보조해주는 역할은 하는 스레드를 데몬 스레드 라 부른다.

메인 스레드 종료 시 같이 종료된다.

ex.  GC(Garbage Collector), 자동저장, 화면 자동갱신

## <mark style="color:yellow;">스레드를 만드는 방법을 나열해주세요.</mark>

1. Thread 클래스를 확장하기.

```
public class prac_thread extends Thread {
    @Override
    public void run() {
        System.out.println("true = " + true);
    }

}

.
.
.


public class Main {
    public static void main(String[] args) {
        
        prac_thread prac_thread = new prac_thread();
        
        prac_thread.start();
    }
}




```

2. Runnable 인터페이스 구현하기

<figure><img src="../.gitbook/assets/image (28).png" alt=""><figcaption></figcaption></figure>

Runnable 은 인터페이스로 run() 메서드가 추상메서드로 잡혀있기에 Thread 클래스 처럼  오버라이딩은 아니지만 run()  메서드를 필수적으로 구현해줘야한다.

```
public class prac_runnable implements Runnable{

    @Override
    public void run() {

        System.out.println("true = " + true);

    }
}

. 
.
.

public class Main {
    public static void main(String[] args) {

        prac_runnable pracRunnable = new prac_runnable();

        Thread thread = new Thread(pracRunnable);

        thread.start();
    }
}
```

Thread 클래스 와 다르게 Runnable 타입 객체를 만들어 준뒤 Thread 객체 new 연산자 파라미터에 Runnable 타입 객체를 집어넣는 방식으로 실행 시킨다.



Thread 클래스는 상속받아 해당 클래스를 확장시키는 방법

Runnable 은 인터페이스 이므로 구현하는 방법



대부분 사람들은 Runnable 인터페이스를 구현하여 Thread 객체에 넣는 방식을 주로 사용한다.

&#x20;이유는 자바에서는 다중상속이 불가능 하기때문에 상속보단 인터페이스를 구현하는게 추후 확장성과 재사용성도 높여주기 때문이다.

## <mark style="color:yellow;">Runnable과 Callable의 차이는 무엇인가요?</mark>

차이점&#x20;

Runnable 은 반환값이 없지만  Callable은 반환값이 있다.

Runnable 은 run() 메서드를 구현하지만 Callable 은  call () 메서드를 구현한다.

Runnable 은 Runnable 객체를 바로 Thread 에 인자로 넣고 실행할수있지만 Callable은  Thread 에 바로 넣진 못하고  Executors 를 통해 쓰레드 풀을 만들고 submit() 메서드에 callable 객체를 담아주면 된다.

submit() 메서든 지정한때에 실행된다. 바로 실행되는 것이 아니기 때문에 바로 반환값을 받을수가 없다.

그렇기에 Future 인터페이스에서  get() 메서드를 이용해서 미래의 결과값을 가져와야한다.

Runnable 은 예외가 없지만 Callable 은 예외를 던진다.

## Callable & Executors 관계

Executors 는 쓰레드 풀을 구현하기 위한 인터페이스 이다.

등록된 작업을 실행하는 책임을 가지고있다. 하지만 Runnable 객체 만을 받는다.



**ExecutorService** &#x20;

* Executor 의 생명주기 관리
* Callable을 작업으로 사용하기 위한 메서드

작업을 등록하는 책임,    등록된 작업을 실행하는 책임을 가지고있다.

.submit (Callable) 의 반환값(future) 에 .get () 을 하여 결과값을 반환받음

```
Future<String> future = executorService.submit(callable);

    System.out.println(future.get());

    executorService.shutdown();
}
```



## <mark style="color:yellow;">스레드의 여러가지 상태에 대해 말해주세요.</mark>



<table><thead><tr><th>상태</th><th>열거 상수</th><th>설명</th><th data-hidden></th><th data-hidden></th></tr></thead><tbody><tr><td>객체 생성</td><td>NEW</td><td>스레드 객체 생성, 하지만 아직 Start() 는 실행 되지 않은 상태</td><td></td><td></td></tr><tr><td>실행 대기</td><td>RUNNABLE</td><td>실행 대기 상태로서 언제든지 실행 가능한 상태</td><td></td><td></td></tr><tr><td>일시정지</td><td>WAITING</td><td>다른 쓰레드가 특정 작업을 수행하기를 기다리는 상태</td><td></td><td></td></tr><tr><td></td><td>TIMED_WAITING</td><td>지정된 대기시간 동안 기다리는 상태</td><td></td><td></td></tr><tr><td></td><td>BLOCKED</td><td>모니터락을 기다리는 동안 차단된 상태</td><td></td><td></td></tr><tr><td>종료</td><td>TERMINATED</td><td>쓰레드가 실행이 완료되어 종료한 상태</td><td></td><td></td></tr></tbody></table>



## <mark style="color:yellow;">sleep()과 wait()의 차이는 무엇인가요?</mark>

* Sleep()

지정된 시간동안 잠시 동작을 멈추는 메서드

synchronized 락이 풀리지 않음.

* Wait()

정지 한뒤 notify() 신호가 있을때 까지 대기한다.

synchronized 락이 풀림



## <mark style="color:yellow;">notify()와 notifyAll()의 차이는 무엇인가요?</mark>

* Notify()

WAIT\_SET 에 있는 임의의 스레드를 RUNNABLE 상태로 변환

* NotifyAll()

WAIT\_SET 에 있는 모든 스레드를  RUNNABLE 상태로 변환



## <mark style="color:yellow;">thread run()과 tnread start()의 차이는 무엇인가요?</mark>

* Run()

싱글쓰레드로 작동한다.

쓰레드 생성없이 바로 run() 실행

호출 수 제한이 없다.



* Start()

멀티 쓰레드로 작동한다.

실행 시 쓰레드 생성하고 쓰레드 실행할때 run() 실행

동일 객체에 두번 이상 호술시 오류 던짐





## <mark style="color:yellow;">스레드 풀을 생성할 수 있는 여러가지 방법을 말해주세요.</mark>



Executor, ExecutorService 를 사용하면 된다.



**1, newCachedThreadPool()**

초기스레드 수, 코어스레드 수 0개 최대 스레드 수는 integer 데이터타입이 가질 수 있는 최대 값(Integer.MAX\_VALUE)

스레드 개수보다 작업 개수가 많으면 새로운 스레드를 생성하여 작업을 처리한다.

만약 일 없이 60초동안 아무일을 하지않으면 스레드를 종료시키고 스레드풀에서 제거한다.



**2. newFixedThreadPool(int nThreads)**

초기 스레드 개수는 0개 ,코어 스레드 수와 최대 스레드 수는 매개변수 nThreads 값으로 지정,

이 스레드 풀은 스레드 개수보다 작업 개수가 많으면 마찬가지로 스레드를 새로 생성하여 작업을 처리한다.

만약 일 없이 놀고 있어도 스레드를 제거하지 않고 내비둔다. &#x20;



## <mark style="color:yellow;">스레드 풀에서 submit()과 execute()의 차이는 무엇인가요?</mark>

* submit()

스레드를 등록하는 메서드드

* execute()

등록된 스레드를 실행하는 메서드



## <mark style="color:yellow;">자바 프로그램에서 멀티 스레드 작업의 안전성을 어떻게 보장할 수 있을까요?</mark>



Synchronized (동기화)

