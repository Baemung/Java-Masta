## 🎯 목표
### 자바의 Annotation, I/O, Generic, Lambda에 대해 학습하기.

## 📌 학습할 것
### [Annotation](#annotation-1)
- [정의하는 방법](#-정의하는-방법)
- [애노테이션의 용도](#-애노테이션의-용도)
- [@Retention](#-retention)
- [@Target](#-target)
- [@Documented](#-documented)
- [Annotation Processor](#-annotation-processor)

### [I/O](#io-1)
- [Stream](#-stream)
- [표준 Stream](#-표준-stream)
- [Channel](#-channel)
- [Buffer](#-buffer)
- [InputStream](#-inputstream)
- [OutputStream](#-outputstream)
- [파일 읽고 쓰기](#-파일-읽고-쓰기)

### [Generic](#generic-1)
- [BoundedType](#-boundedtype)
- [WildCard](#-wildcard)
- [Erasure](#-erasure)

### [Lambda](#lambda-1)
- [Lambda 사용법](#-lambda-사용법)
- [함수형 인터페이스](#-함수형-인터페이스)
- [Variable Capture](#-variable-capture)
- [메소드, 생성자 레퍼런스](#-메소드-생성자-레퍼런스)

--- 

## Annotation

**`애노테이션(Annotation)`** : 사전적 의미로 주석을 의미하며, 프로그램에 대한 데이터를 제공하는 메타데이터의 한 형태

---

### 💡 정의하는 방법

애노테이션 타입은 java.lang or java.lang.annotation에서 제공해주는 것이 있고 필요에따라 자신이 직접 정의해서 사용할 .

애노테이션을 직접 정의하는 방법은 `@interface` 키워드를 사용하여 정의한다.

```java
public @interface CustomAnnotation {
    	// Element
	int value();
}

@CustomAnnotation(value = 29)
@CustomAnnotation(29)
```

`@interface`는 애노테이션 타입(annotation type)을 선언하는 키워드이고, 애노테이션 타입 선언을 일반적인 인터페이스 선언과 구분하기 위해 interface 앞에 기호 @를 붙인다.

애노테이션은 Element라는 것을 멤버로 가질 수 있으며, Element는 타입과 이름으로 구성되며 default 값을 가질 수 있다. 

Element의 이름 뒤에는 메소드를 작성하는 것 처럼 뒤에 ()를 붙여야 한다.

정의된 애노테이션을 사용하는 방법은 `@애노테이션` 명으로 선언할 수 있고, 특정 클래스나 메소드, 변수에 붙여 사용한다.

그리고 한 선언부에 여러 애노테이션을 사용하는 것도 가능하다.

기본 Element는 value이며, 해당하는 값은 생략하고 바로 사용이 가능하다.

---

### 💡 애노테이션의 용도

1. **`컴파일 타임`에 제공하는 정보** 
	- `@Override` : 상위 클래스의 메서드를 재정의하였음을 나타내는 Annotation. 상위 클래스에 해당 메서드가 존재하지 않는 경우 Compiler가 경고하는 기능을 제공함.
	- `@SuppressWarnings` : 컴파일러가 경고하는 내용을 Ignore 할 때 사용하는 Annotation
	- `@Generated` : 소스 코드를 생성하는 도구에 의해서 해당 코드가 자동적으로 생성되었음을 나타내는 Annotation
2. **컴파일 시간 및 배포 시간 처리** -> 소프트웨어 개발툴이 애노테이션 정보를 처리하여 코드, XML 파일등을 생성 가능 (애노테이션을 사용한 곳에 특정 코드를 추가 ex) Lombok의 `@Getter`, `@Setter`)
3. **런타임 처리** -> 일부애 노테이션은 런타임에 특정 기능을 실행하도록 정보를 제공 (Java Reflection)
	- `자바 리플렉션(Java Reflection)` : 구체적인 클래스 타입을 알지 못해도 런타임 시에 클래스 이름만 알고있다면 그 클래스의 메소드, 타입, 변수들에 접근할 수 있도록 해주는 자바 API, 
	- `@FunctionalInterface` : 해당 Interface가 하나의 메서드만을 제공하는 함수형 인터페이스임을 나타내는 Annotation
	- `@Deprecated` : 더 이상 지원하지 않는 기능임을 알려주는 Annotation
	- `@SafeVarargs` : Generic과 같은 가변 인자 Parameter를 사용할 때 발생하는 경고를 ignore 하는 Annotation
	- `@Repeatable` : 해당 Annotation을 반복적으로 사용할 수 있게 지원하는 Annotation
	- `@Inherited` : Meta Annotation 중 하나로 써 적용된 대상 Annotation을 하위 Class, Interface에게 상속시키는 Annotation

---

### 💡 @Retention

`@Retention` 에노테이션은 애노테이션 선언에 사용되는 메타 애노테이션으로, 마크된 에노테이션을 언제까지 유지할 건지 결정해주는 애노테이션이다.

즉, Java 컴파일러가 애노테이션을 다루는 방법을 기술하며, 어느 시점까지 영향을 미치는지 결정한다. 

보존할 기간을 특정하게 명시하지 않으면 기본값은 클래스 파일까지 보존한다.

해당 정책들은 `java.lang.annotation.RetentionPolicy`에 `enum`으로 정의되어 있으며 아래와 같다.

- `SOURCE` : 컴파일러가 컴파일할 때 해당 애노테이션 제외
- `CLASS` : 컴파일러가 컴파일에서는 해당 애노테이션을 가져가지만 런타임 시에는 사라짐
- `RUNTIME` : 런타임에도 해당 애노테이션 유지

```java
@Retention(RetentionPolicy.RUNTIME) //런타임에도 해당 애노테이션 유지
public @interface CustomAnnotation {
    	// Element
	int value();
}
```

---

### 💡 @Target

`@Target` 애노테이션은 애노테이션 선언에 사용되는 메타 애노테이션으로, 애노테이션을 적용할 수 있는 범위를 정의한다.

해당 범위들은 `java.lang.annotation.ElementType`에 다양한 범위가 정의되어 있으며 아래와 같다.

- `TYPE` : Class, Interface 등의 Level에 선언 가능
- `METHOD` : 메서드에 선언 가능
- `FIELD` : Enum, 상수, Field 변수에 대해 선언 가능
- `PARAMETER` : 매개변수에 선언 가능
- `CONTRUCTOR` : 생성자 Type에 선언 가능
- `LOCAL_VARIABLE` : 지역 변수에 대해 선언 가능
- `ANNOTATION_TYPE` : Annotation에 선언 가능
- `PACKAGE` : 패키지에 선언 가능

```java
@Target(ElementType.FIELD) //  Enum, 상수, Field 변수에 대해 선언 가능
@Retention(RetentionPolicy.RUNTIME) // 런타임에도 해당 애노테이션 유지
public @interface CustomAnnotation {
    	// Element
	int value();
}
```

---

### 💡 @Documented

`@Documented` 애노테이션은 애노테이션 선언에 사용되는 메타 애노테이션으로, 해당 애노테이션에 대한 정보를 Javadoc문서에 포함한다는 것을 선언한다.
 
```java
@Documented // 이 애노테이션에 대한 정보를 Javadoc문서에 
@Target(ElementType.FIELD) //  Enum, 상수, Field 변수에 대해 선언 가능
@Retention(RetentionPolicy.RUNTIME) // 런타임에도 해당 애노테이션 유지
public @interface CustomAnnotation {
    	// Element
	int value();
}
```

---

### 💡 Annotation Processor

`Annotation Processor`는 애노테이션을 이용하여 프로세스를 처리하는 것을 의미하며, 컴파일러가 컴파일 중에 새로운 소스코드를 생성하거나 기존의 코드 변경을 가능하게 한다.

특히 컴파일 단계에서 애노테이션에 정의된 액션을 처리하는데, 이것은 애플리케이션이 실행되기 전 컴파일 타임에 미리 체크를 해주기 때문에 애노테이션이 의도한대로 이루어지지 않으면 눈으로 확인할 수 있는 에러나 경고를 보여준다.

애노테이션 프로세서의 가장 대표적인 예로는 `Lombok` 라이브러리가 있다. 

Getter, Setter와 같이 반복적이고 중복되는 많은 코드를 양산하는 코드(boilerplate code)를 최소화하고 핵심적인 기능을 명확하게 확인할 수 있도록 한다.

애노테이션 프로세서를 작성하고 등록하기 위해서는 `AbstractProcessror` 클래스를 상속받아야 하는데, `Lombok` 또한 `AbstractProcessor`를 상속받아 애노테이션 프로세서를 정의한 것이다. 
 
1. `Lombok`
	- **@Getter** **@Setter** **@Builder**
2. `AutoService` : 
 	- **java.util.ServiceLoader** 용 파일 생성 유틸리티
	- 리소스 파일을 만들어 줌.
	- META-INF 밑의 service 밑에 ServiceLoader용 레지스트리 파일을 만들어 줌.
3. `@Override`
	- 컴파일러가 오버라이딩하는 메서드가 잘못된 대상임을 체크해주는 것도 애노테이션 프로세서임
4. `Dagger2`
	- 컴파일 타임 DI 제공 -> 런타임 비용이 없어짐.
5. `안드로이드 라이브러리` 
	- **ButterKnife** : @BindView (뷰 아이디와 애노테이션 붙인 필드 바인딩)
	- **DeepLinkDispatcher** : 특정 URI 링크를 Activity로 연결할 때 사용

---

## I/O

`I/O`는 Input(입력) and Output(출력)의 약자이다.

I/O의 간단한 예시는, 키보드로 텍스트를 입력하고 모니터로 입력한 텍스트를 출력하는 것이다.

기존의 `IO(I/O)`방식은 스트림 방식의 입출력인데, 이 방식으로는 네트워크상에서 이루어지는 입출력의 속도가 하드웨어에서 이루어지는 입출력 속도에 비해 현저히 느리기 때문에 문제점이 생겼다.

또한 스트림 방식은 병목현상에 매우 취약하다는 단점이 있고 네트워크의 발전보다 하드웨어의 발전속도가 앞서나가면서 새로운 I/O 방식이 필요해졌다.

그래서 이러한 네트워크 환경에서의 문제점을 해결하기 위해서 `NIO(NEW I/O)`방식이 나타났다.

|구분|IO|NIO|
|:-:|:-:|:-:|
|입출력|스트림|채널|
|버퍼|non-버퍼|버퍼|
|비동기|지원 안함|지원|
|블로킹|블로킹만 지원|블로킹 / non-블로킹 모두 지원|

---

### 💡 Stream

Stream이란, 한 방향으로 연속적으로 흘러가는 것을 의미한다.

![Untitled](https://user-images.githubusercontent.com/51703260/134504606-d51eed4a-a9bd-41f8-9d45-4050e46ea012.png)

이 Stream이 프로그래밍에서는 데이터가 한 방향으로 흘러갈 수 있도록 도와주는 관, 통로라는 의미로 사용된다.

입출력 노드에 직접 연결되는 Stream을 `노드스트림`이라고 하고, 이는 **필수** 다.

그리고 다른 스트림을 보조하는 기능적인 Stream을 `필터스트림`이라고 하며, 이는 **옵션** 이다.

`데코레이터 패턴`으로 `노드스트림`에 `필터스트림`이 추가되는 방식이다. 

<br>

Stream이 데이터를 어떤 방식으로 전달하느냐에 따라 2가지로 구분할 수 있다.

1. **`Byte Stream` (바이트 스트림)**
    - 데이터를 Byte 단위로 주고 받음
    - Binary 데이터를 입출력하는 스트림
    - **모든 종류의 데이터** 를 주고받을 수 있음
    - `InputStream`과 `OutputStream` 클래스를 상속받아 사용
    - ---
    - `FileInputStream` / `FileOutputStream` : 파일 입출력 스트림
    - `ByteArrayInputStream` / `ByteArrayOutputStream` : 메모리 입출력 스트림
    - `PipedInputStream` / `PipedOutputStream` : 프로세스 입출력 스트림
    - `AudioInputStream` / `AudioOutputStream` : 오디오 장치 입출력 스트림

자바에서 가장 작은 타입인 char 형이 2바이트이므로, 1바이트씩 전송되는 바이트 기반 스트림으로는 원활한 처리가 힘든 경우가 있다. 이러한 경우를 해결하기 위해 자바는 `Character Stream`을 지원함

2. **`Character Stream` (문자 스트림)**
    - 데이터를 Character 단위로 주고 받음
    - 문자 데이터를 인코딩 처리하여 입출력하는 스트림
    - 오직 **문자 데이터** 만 주고받을 수 있음
    - `Reader`와 `Writer` 클래스를 상속받아 사용
    - ---
    - `FileReader` / `FileWriter` : 파일 입출력 스트림
    - `CharArrayReader` / `CharArrayWriter` : 메모리 입출력 스트림
    - `PipedReader` / `PipedWriter` : 프로세스 입출력 스트림
    - `StringReader` / `StringWriter` : 문자열 입출력 스트림

두 Stream은 모두 처음에는 byte로 받아들이고, 그 다음은 각 Stream이 알아서 처리를 해준다.

---

### 💡 표준 Stream

`System` Class는 자바에서 미리 정의해둔 표준 입출력 클래스이며, `java.lang` 패키지에 포함되어 있다.

- `System.in` : 표준 입력 스트림
- `System.out` : 표준 출력 스트림
- `System.err` : 표준 에러 출력 스트림

---

### 💡 Channel

`서버와 클라이언트간의 통신수단`을 의미한다. 

일반적으로 NIO(new I/O)의 모든 I/O는 채널로 시작한다. 

채널은 스트림과 유사하지만 몇 가지 차이점이 존재한다.
- **비동기적** 입출력
- `스트림`은 단방향 **입출력(읽기 또는 쓰기)** 만 가능하지만 `채널`을 통해서는 **양방향 입출력(읽고 쓰기)** 이 가능하다 있다.
- 항상 `버퍼`를 이용하여 입출력을 한다.

---

### 💡 Buffer

**데이터를 임시 저장하는 공간**을 의미한다.

`IO`에서는 출력 스트림이 1바이트를 쓰면 입력 스트림이 1바이트를 읽는다.

`NIO`에서 버퍼는 채널과 상호작용할 때 사용된다. 커널에 의해 관리되는 시스템 메모리를 직접 사용할 수 있는 채널에 의 해 직접 read 되거나 write 될 수 있는 배열과 같은 객체이다.

입출력을 담당하는 장치, 프로세스에서 고속 장치(프로세스)가 저속 장치(프로세스)가 작업을 하는 동안 기다리는 시간을 줄여 개별 작업들 간의 협동을 원활하게 지원하기 위해 존재한다.

따라서, 1바이트씩 전달이 아니라 기다리는 동안 버퍼에 데이터를 축척하여 한번에 전달하여 빠른 속도를 보인다.

---

### 💡 InputStream

`Byte Stream`기반의 입력 스트림의 최상위 클래스(추상클래스)로써 모든 Byte기반 입력 스트림은 이 클래스를 상속받고 데코레이터 패턴으로 기능을 추가하는 방식으로 스트림이 만들어진다.

`InputStream` 클래스에는 입력을 위해 필요한 기본적인 메소드들이 정의되어 있다.

- `int available()` : 현재 읽을 수 있는 byte수를 리턴
- `abstract int read()` : InputStream으로부터 1byte를 읽고 리턴.
- `int read(byte[] b)` : InputStream으로부터 읽은 바이트들을 byte[] b에 저장하고 읽은 바이트 수를 리턴.
- `int read(byte[] b, int off, int len)` : InputStream으로부터 **len** byte만큼 읽어 byte[] b의 b[ **off** ] 부터 **len** 개까지 저장한 후 읽은 byte 수인 **len** 개를 리턴. 만약 **len** 개보다 적은 byte를 읽는 경우 실제 읽은 byte수를 리턴
- `void close()` : 사용한 시스템 리소스를 반납 후 입력 스트림을 닫는 메소드.close() : 사용한 시스템 리소스를 반납 후 InputStream을 닫음.

---

### 💡 OutputStream

OutputStream 또한 `Byte Stream`기반의 출력 스트림의 최상위 클래스(추상클래스)로써 모든 Byte기반 출력 스트림은 이 클래스를 상속받고 데코레이터 패턴으로 기능을 추가하는 방식으로 스트림이 만들어진다.

`OutputStream` 클래스에도 출력을 위해 필요한 기본적인 메소드들이 정의되어 있다.

- `void flush()` : 버퍼에 남아있는 OutputStream을을 출력
- `abstract void write(int b)` : 정수 b의 하위 1byte를 출력
- `void write(byte[] b)` : 버퍼의 내용을 출력
- `void write(byte[] b, int off, int len)` : b 배열 안의 시작점 **off** 부터 **len** 만큼 출력
- `void close()` : 사용한 시스템 리소스를 반납 후 OutputStream을 닫음.

---

### 💡 파일 읽고 쓰기

자바의 내장 클래스인 `FileReader`, `FileWriter`, `BufferedReader`, `BufferedWriter`를 사용하여 파일을 읽고 쓸 수 있다.

```java
File file = new File(PATH);

FileWriter fw = new FileWriter(file);
FileReader fr = new FileReader(file);
BufferedWriter bw = new BufferedWriter(fw);
BufferedReader br = new BufferedReader(fr);
```

`FileReader`, `FileWriter`와 `BufferedReader`, `BufferedWriter`는 데코레이터 패턴이 적용됨을 알 수 있다.

--- 

## Generic

`Java 5`부터 추가되어 클래스와 인터페이스, 메소드를 정의할 때 `타입 변수`를 사용할 수 있게 하는 방법이다.

- **`타입 변수`** : 일반적으로 대문자 알파벳 한 글자로 표현한다.
  - `<T>` :	Type
  - `<E>` :	Element
  - `<K>` :	Key
  - `<N>` :	Number
  - `<V>` :	Value
  - `<R>` :	Return

컴파일 시의 객체의 타입을 체크를 해주는 기능을 제공함으로써 안정성을 높이고 형변환의 번거로움을 줄여준다.

`Collection` 라이브러리에서 흔히 `Generic`이 활용된다.
```java
//List<T>
List<Integer> listG = new ArrayList<>(); // 제네릭
listG.add(1);
int temp = listG.get(0);

List listNG = new ArrayList(); // 제네릭 사용 X
listNG.add(1);
temp = (int)listNG.get(0); // 형변환 필요
```

Generic은 `Generic 타입`과 `Generic 메소드`로 구분할 수 있다.

#### `Generic 타입`

타입 변수가 있는 클래스 또는 인터페이스

```java
class GenericClass<T> {
     ...
}

interface  GenericInterface<T> {
     ...
}
```

#### `Generic 메소드`

매개 타입과 리턴 타입으로 타입 변수를 갖는 메소드

```java
public <T, R> R genericMethod(T t) { }
```

#### 제네릭을 사용할 수 없는 경우

1. **제네릭 타입의 배열을 생성할 경우** : new T[N]은 컴파일 타임에 배열의 타입을 알 수 없기 때문에 사용할 수 없다.

2. **static 변수에 사용할 경우** : static 변수는 인스턴스에 종속되지 않으므로 인스턴스별 타입이 변경될 수 없기 때문에 사용할 수 없다. (단, static 메소드에는 제네릭 사용 가능)

<br>

Generic의 주요 개념으로는 `Bounded-Type`과 `Wild-Card`가 있다.

---

### 💡 BoundedType

`Bounded-Type`은 제네릭으로 사용될 타입 변수의 범위를 제한하는 것이다.

Generic 타입에 `extends`를 사용하여 타입 변수의 범위를 제한한다. (인터페이스, 클래스 모두 상관없이 `extends`를 사용)

```java
public class CustomGeneric<T extends Number> {
     ...
}

CustomGeneric<Integer> listI = new CustomGeneric<();
CustomGeneric<String> listS = new CustomGeneric<>(); // Bounded-Type으로 타입 변수를 Number로 제한했기 때문에 컴파일 에러 발생!
```

```java
public class CustomGeneric<T extends ClassName & InterfaceName> { // & 기호를 사용하여 타입 변수를 클래스와 인터페이스 2개로 제한 가능 
     ...
}
```

---

### 💡 WildCard

제네릭 타입을 메소드의 매개값으로 전달할 때 구체적인 타입으로만 타입 제한이 생긴다 그러한 문제를 해결하기 위해 와일드 카드를 사용한다.

`?` 키워드를 사용하여 와일드 카드를 사용할 수 있다.

- `제네릭 타입 <?>` : 제한 없음. 모든 클래스나 인터페이스 가능
- `제네릭 타입 <? extends 상위 타입>` : 타입의 상한선 지정(해당 타입 및 하위타입만, 최상이 제시한 타입), 최대 제시된 상위 타입보다 더 상위 타입을 사용할 수 없음
- `제네릭 타입 <? super 하위 타입>` : 타입의 하한선 지정(해당 타입 및 상위타입만, 최하가 제시한 타입), 최소 제시된 하위 타입보다 더 하위 타입을 사용할 수 없음

---

### 💡 Erasure

- `제네릭의 타입 소거(Generics Type Erasure)` : 컴파일러는 제네릭 타입을 이용해 소스파일을 검사하고 런타임에는 해당 타입의 정보를 알 수 없다는 개념이다. 즉, 컴파일된 파일`*.class`에는 제네릭 타입에 대한 정보가 없다는 뜻.

#### 💡 Erasure 규칙
- `UnBounded-Type`은 Object
  - ex) `(<?>, <T>)` => Object
- `Bounded-Type`은 extends 뒤에 작성한 객체
  - ex) `<T extends Number>` => Number

---

## Lambda

람다식(Lambda expression)은 메소드를 간단하면서 명확한 `식(expression)`으로 표현한 것이다.

람다식은 인해 이름과 리턴타입이 필요없어, `익명 함수(anonymous function)`라고도 한다.

람다식은 메소드와 동등한 것처럼 보이지만, 사실 람다식은 익명 클래스의 객체와 동등하여 런타임 시에 인터페이스의 익명 구현 객체로 생성되고 대입되는 인터페이스에 따라 구현 인터페이스가 결정된다.

Lambda의 도입으로 인해 자바는 객체 지향 언어인 동시에 함수형 언어가 되었다.

---

### 💡 Lambda 사용법

메소드에서 메소드이름과 리턴타입을 제거하고 `매개변수` `{실행코드}` 사이에 `->` 화살표를 추가하여

`매개변수` -> `{실행코드}` 로 람다식을 사용할 수 있다.

메소드를 선언 할 때도 끝에 `세미콜론 ;`을 붙이지 않듯이, 람다식 또한 마찬가지로 `세미콜론 ;`을 붙이지 않는다 

```java
// 메소드
리턴타입 메소드이름 (매개변수타입 매개변수) {
    실행코드
}
// 람다식
((매개변수) -> {실행코드})
// 매개변수 :
// 매개변수가 하나일 때는 ()를 생략할 수 있다.   
// 단, 매개변수 타입이 있으면(대부분의 매개변수 타입은 추론이 가능해서 생략 가능) ()를 생략할 수 없다.  
// 코드 : 
// 실행코드가 하나일 때는 {}를 생략할 수 있다.   
// 단, {} 안의 실행코드에 return 키워드가 사용되는 경우 {}를 생략할 수 없다. 


// 메소드
int max(int a, int b) {
    return a > b ? a : b;
}
// 람다식
((a, b) -> a > b ? a : b) // 매개변수가 a와 b 2개이므로 () 생략 불가. 그리고 return 키워드가 사용되지 않으므로 {} 생략 가능
```

List를 람다식으로 조작하는 간단한 예시를 살펴보자.

```java
ArrayList<Integer> list = new ArrayList<Integer>();
for(int i=1; i<=10; ++i) list.add(i);
		
list.replaceAll(e -> e+10); // 모든 요소에 10씩 더하기
list.removeIf(e -> e%2==1); // 홀수값 요소 없애기
list.forEach(e -> System.out.println(e)); // 모든 요소 출력	
```

그리고 PS를 하다보면 자주 접하게 되는 Comparator 인터페이스의 compare메소드 또한 익명 구현 객체로 사용할 수도 있지만, 람다식으로 간단하게 표현할 수 있다. 
```java
// Comparator 인터페이스의 compare메소드 익명 구현 객체
Arrays.sort(time, new Comparator<int[]>() {
			@Override
			public int compare(int[] t1, int[] t2) {
				if(t1[0] == t2[0]) return t1[1] - t2[1];
				else return t1[0] - t2[0];
			}
		});
```

```java
// 람다식
Arrays.sort(time, (t1, t2)->{
			if(t1[0] == t2[0]) return t1[1] - t2[1];
			else return t1[0] - t2[0];
		});
```

---

### 💡 함수형 인터페이스

람다 표현식으로 구현이 가능한 인터페이스는 추상 메서드를 1개만 가지고 있는 인터페이스만 가능하다.

그래서 추상 메서드가 1개인 인터페이스를 함수형 인터페이스라고 한다.

```java
// 약식 사용 예제
@FunctionalInterface
interface CustomFunction{
    void customMethod();
}

// 사용 1. 람다식을 참조하는 참조변수로 함수형 인터페이스를 지정하고, 참조변수에서 메소드를 호출
CustomFunction cf = () -> System.out.println("This is FunctionalInterface");
customMethod(cf); // This is FunctionalInterface

// 사용 2. 참조변수 없이 바로 람다식을 메소드로 사용 
customMethod() -> System.out.println("This is FunctionalInterface");
```

```java
// 매개변수를 받고 리턴해주는 예제
@FunctionalInterface
interface CustomFunction{
    int customMethod(int a, int b);
}

CustomFunction cf = (a, b) -> a > b ? a : b;
int bigNum = cf.customMethod(1, 2); 

```

`@FunctionalInterface` : 해당 인터페이스가 함수형 인터페이스라는걸 알려주는 애노테이션, 인터페이스 선언시 해당 애노테이션을 붙이면 2개 이상의 추상 메소드가 선언되지 않았는지 컴파일러가 체크하여 2개 이상의 추상 메소드가 선언되어 있다면 컴파일 에러가 발생한다.

모든 애노테이션들의 기능이 그렇듯, `@FunctionalInterface`은 컴파일러에게 확인을 부탁하는 것일 뿐 안붙어있다고 해서 함수형 인터페이스로 동작하지 않는 것은 아니다. 

자바에서 함수형 인터페이스를 제공하는 [표준 API](https://docs.oracle.com/javase/8/docs/api/java/util/function/package-summary.html) `java.util.function`를 제공해준다.

---

### 💡 Variable Capture

`Variable Capture`는 람다식에서 파라미터로 넘겨진 변수가 아닌 외부에서 정의된 변수를 참조하는 것을 의미한다.

람다식에서 접근 가능한 변수들은 아래와 같다.

1. `로컬 변수`   
2. `static 변수` 
3. `인스턴스 변수` 

`Variable Capture`에는 위 변수들 중 1. `로컬 변수`에 대한 제약 조건이 존재한다.

1. 로컬 변수는 `final`으로 선언되어야 한다,
2. `final`로 선언되어 있지 않은 로컬 변수는 `Effectively Final`(유사 final; final 처럼 동작)이어야 한다. 즉, 값의 재할당이 일어나면 안된다.

따라서, 람다식에서 로컬 변수만 변경이 불가능하고, 나머지 변수들은 읽기 및 쓰기가 가능하다.

이러한 제약이 존재하는 이유는 아래와 같다.

>1주차에서 정리하였듯이, JVM에서 로컬 변수는 스택 영역에 생성된다.
>
>실제 메모리와는 달리 JVM에서 스택 영역은 쓰레드마다 별도의 스택이 또 생성되어 쓰레드끼리는 로컬 변수 공유가 불가능 하다.
>
>그리고 JVM에서 인스턴스 변수는 힙 영역에 생성되기 때문에 인스턴스 변수는 쓰레드끼리 공유가 가능하다.
>
>람다식은 로컬 변수가 존재하는 스택에 직접 접근하지 않고, 로컬 변수를 자신(람다가 동작하는 별도의 쓰레드)의 스택에 복사한다. 
>
>그래서 만약 원래 지역 변수가 있는 쓰레드는 사라져서 로컬 변수가 사라져도 에러가 발생하지 않는다.
>
>그래서 진짜 문제는 멀티 쓰레드 환경에서, 여러 개의 쓰레드에서 람다식을 사용하면서 람다 캡쳐링이 계속 발생하는데 이 때 외부 변수 값의 불변성을 보장하지 못한다면 동기(sync)화 문제가 발생할 수 있다.
>
>이러한 문제 때문에 지역변수에 대한 제약사항이 존재하게 되는 것이다.
>
>static 변수나 인스턴스 변수는 스택 영역이 아닌 힙 영역에 위치하고, 힙 영역은 모든 쓰레드가 공유하고 있는 메모리 영역이기 때문에 값을 직접 접근하기 때문에 문제가 없는 것이다.

---

### 💡 메소드, 생성자 레퍼런스

#### `Method Reference` 

람다식이 하나의 메소드만 호출하는 경우에는 '메소드 참조(Method References)'를 통해 람다식을 더 간략하게 표현할 수 있다.

메소드를 참조해서 매개 변수의 정보 및 리턴 타입을 알아낸다. 람다식에서 불필요한 매개 변수를 제거하는 것이 목적이다.

> ClassName::MethodName
>
> or
>
> ReferenceVariable::MethodName

```java
Integer parseInt(String s) { 				// 메소드
    return Integer.parseInt(s);
}
Function<String, Integer> f = s -> Integer.parseInt(s); // 람다식
Function<String, Integer> f = Integer::parseInt; 	// 메소드 참조
```

```java
Function<String, String, Boolean> f = (s1, s2) -> s1.equlas(s2); 	// 람다식
Function<String, String, Boolean> f = String::equals; 			// 메소드 참조
```

#### `Constructor References`

`생성자 참조(Constructor References)`는 Constructor를 생성해주는 코드로써 객체 생성을 의미한다.

> ClassName::new

```java
Function<Integer, int[]> f = x -> new int[x];   // 람다식
Function<Integer, int[]> f = int[]::new;  	// 생성자 참조
```

```java
Supplier<CustomClass> s = () -> new CustomClass(); 	// 람다식
Supplier<CustomClass> s = CustomClass:new; 		// 생성자 참조
```
