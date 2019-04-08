---
layout: post
title: "Junit 관련 내용"
categories: Junit
tags: Junit
---
* content
{:toc}
TemporaryFolder API

https://qiita.com/tag1216/items/4b8ebee07ebcf8618d7d

폴더를 생성하고, 갱신하는 클래스를 테스트할 때에는, 다음 실행할 때와 다른 테스트의 실행에 영향이 없도록, 뒷 처리로 작업 디렉토리를 원래 상태로 돌리지 않으면 안된다.

 

이러한 때에는 org.junit.rules.TemporaryFolder을 사용하면, 테스트메소드을 실행할 때마다 

함께 디렉토리를 작성하고 종료할 때 에는 자동적으로 제거되도록 된다.

 

사용법

테스트클래스에 TemporaryFolder형의 public폴더를 작성하고, @Rule 어노테이션을 붙여두면, 테스트메소드가 실행될 때마다 함께 디렉토리를 생성해 준다.

 

@Rule

public TemporaryFolder tempFolder = new TemporaryFolder();

 

생성시킨 일시적 디렉토리 패스는 getRoot()로 취득 할 수 있다.

//getRoot()는 임시폴더의 위치

tempFolder.getRoot();

 

일시 디렉토릭내에 폴더랑 서브디렉토리를 생성할 때에는 createFile() 과 createFolder()를
 용한다.

인수가 없는 쪽을 사용하면 랜덤인 이름을 붙여 작성해준다.

//인수 명으로 작성

tempFolder.createFile(filename);

tempFolder.createFolder(foldername);

//랜덤

tempFolder.createFile();

tempFolder.createFolder();

 

테스트용 파일을 사용목적으로 있는 경우에는, 일시 디렉토리에 파일을 카피하고 나서 사용하면, 파일을 갱신/제거하려는 테스트라도, 다음 실행때에 영향이 없다. 

 

FileUtils.copyDirectory(new File(“src/test/data/test1/”), tempFolder.getRoot());

//첫번째 인수내용을 두번째 인수 내용에 복사한다는 것을 의미한다.

****모든 디렉토리와 파일을 지정하는데, 성공 실패에도 아무런 표시가 나타나지 않는다.

 

https://cafe.naver.com/infoguide114/280

getAbsolutePathName(pathspec) 란,

이 추상의 메서드명의 절대 패스명 문자열로 반납한다.

제공된 경로 내역에 근거하여 완전하고 분명한 경로를 반환한다.

pathspec : 필수적인 요소, 완전하고 분명한 경로로 바꿀 경로 내역이다.

 

현재의 유저의 시스템 프로퍼티의 유저 디렉토리로 표시된다.            

 

Pathspec                      반환되는 경로

"c:"                             "c:\mydocuments\reports" 

"c:.."                           "c:\mydocuments" 

"c:\\\"                     "c:\" 

"c:*.*\may97"              "c:\mydocuments\reports\*.*\may97" 

"region1"                      "c:\mydocuments\reports\region1" 

"c:\..\..\mydocuments" "c:\mydocuments" 

 

 

File.separator

자바는 이클립스 기반으로 개발을 많이 하게 된다. 유연한 프로그램을 만들기 위해서는 프로퍼티 파일을 사용하는 것이 일반적인데, 프로퍼티 파일에 특정 파일 경로는 거의 필수적으로 포함된다 보면 된다. 하지만 경로를 나타내는 구문문자가 UNIX와 윈도우 각 OS에 차이로 다르게 움직인다 이것을 해결하기 위한 것이 File.separator이다.

 

사용방법

예) Data 디렉토리 안에 readme.txt 파일을 원한다 할 때,

윈도우 Data\\readme.txt

리눅스 Data/readme.txt

가 된다.

 

하지만 자바에서는

“Data” + File.separator + “readme.txt”

라고 이용하면된다.

출처

<https://shineware.tistory.com/entry/%EC%8B%9C%EC%8A%A4%ED%85%9C%EC%97%90-%EB%94%B0%EB%9D%BC-%EB%8B%A4%EB%A5%B8-Fileseparator>.

 

File 클래스

<https://blog.naver.com/harloveysj/221480763073>

<https://hyeonstorage.tistory.com/233>

<https://docs.oracle.com/javase/jp/7/api/java/io/File.html>

 

Junit annotation

<https://blog.naver.com/dajung674/221376544132>

 

Before, After, BeforeClass, AfterClass

 

@Before / @After는 몇 개씩 있어도 상관없다. Junit은 이들을 빠짐 없이 호출한다.

다만 호출 순서는 보장하지 않는다.

 

@Before

-테스트 메서드 별로 초기화 처리

-각 테스트 수행전 실행되는 메소드이다.

-주로 픽스쳐를 준비한다.

​           *fixture

​           -테스트를 수행하는 데 필요한 정보나 오브젝트이다.

​           -일반적으로 픽스처는 여러 테스트에서 반복적으로 사용되기 때문에

​           @Before 메소드를 이용해 생성해두면 편리하다.

​           -UserDaoTest에서는 dao가 대표적인 픽스처이다.

​           -테스트 중에 add() 메소드에 전달하는 User 오브젝트들도 픽스처라고 볼 수 있다.

-pulbic void 형태를 가진다.

 

@After

-각 테스트 수행 후 실행되는 메소드이다.

-public void 형태를 가진다.

 

@BeforeClass/ @AfterClass 는 테스트 실행, 전후에 전체적으로 한번 씩만 호출하는 메소드이다.

-public static으로 생성한다.

*BeforeClass : 테스트를 통틀어서 단 한번만 초기화하는 작업을 실행한다. 

 

@Test

테스트 대상 메소드를 말한다.

 

Handler

https://qiita.com/Qui/items/40077ce9e33738dd3914

핸들러는 여러 개를 등록할 수 있다. 인스턴스 작성

Logger logger = Logger.getLogger(“logger의 이름”)

Handler handler = new FileHandler(“c:\\sample\\sample.log”);

Logger.addHandler(handler);

 

Formatter formatter = new simpleFormatter();

Handler.setFormatter(formatter);

 

최후에 Formatter 인스터스를 작성하고 핸들러를 등록합니다.

이번에는 사람이 읽기 쉬운 형식으로 포맷하기 위해, simpleFormatter 을 이용합니다. simpleFormattersms formatter의 인터페이스 구현클래스입니다.

이것으로 준비는 끝입니다 logger에 메시지를 건내서 로그를 출력합니다.

 

logger.log(Level.INFO, “message”); 로그래벨을 지정해준다.

•FINEST - 非常に詳細なトレースメッセージ

•FINER - かなり詳細なトレースメッセージ

•FINE - 詳細なトレースメッセージ

•CONFIG - 静的な構成メッセージ

•INFO - 情報メッセージ

•WARNING - 警告メッセージ

•SEVERE - 重大なメッセージ

​                                                                               

Logger.setLevel(level.INFO);

인포 레벨이상의 메시지만 출력할 경우 설정

 

 

METHOD

<https://docs.oracle.com/javase/jp/8/docs/api/>

 

Method 클래스 또는 인터페이스의 메서드를 한 번에 대 한 액세스를 제공 합니다. 반영 된 메서드는 클래스 또는 인스턴스 메소드 (추상 메서드를 포함 하 여)입니다. Method는 실제 매개 변수를 기본 메서드 형식 매개 변수와 일치 하는 경우 확장 변환이 발생할 수 있지만 축소 변환이 발생할 경우 IllegalArgumentException 발생 합니다.

 

getDeclaredMethod(String name, Class <>….. parameterTypes)

이 Class 개체가 나타내는 클래스 또는 인터페이스의 지정 된 선언 된 메소드를 반영 하는 Method 개체를 반환 합니다. Name 매개 변수는 요청 된 메서드는 단순히 이름을 지정 하는 String입니다. ParameterTypes 매개 변수는 메서드의 형식 매개 변수의 형식을 선언 순서 대로 확인 하는 Class 개체의 배열입니다. 동일한 매개 변수 형식을 갖는 메서드를 단일 클래스에 선언 된 이러한 메서드 중 하나가 다른 것 보다 명확한 반환 값의 형식이 있는 경우에는 해당 메서드가 반환 됩니다. 그렇지 않으면 메서드 중 하나가 임의로 선택 됩니다. 

이름이 「<init>또는<clinit>」의 경우, NoSuchMethodException가 발급 됩니다.

이 Class 오브젝트가 배열 형식을 나타내는 경우이 메서드는 clone () 메서드를 찾지 않습니다. </clinit></init>

 

Method.invoke(Object obj, Object… args)

https://blog.naver.com/bajohw/40198155644

특정 Class의 Method를 호출해서 값을 설정해주거나 값을 받아 올 수 있다.

예를 들어 30개의 파라미터를 받아 Bean 객체에 각각 30개를 설정하려고 할 때

 

set메소드를 30번 쭉~ 나열하여 코딩 할 수 없는 노릇이다.

 

프로젝트 진행중에 invoke 메소드를 사용해서 여러 개의 파라미터를 set해주는 기능을 만들어 보았다.

 

https://blog.naver.com/kst7132/140120735523

 

setAccessible(Boolean xx)
 
 

 

Arrays.asList(1, 2, 3, 4)

배열을 리스트에 저장

Arrays.asList(new Integer[]{1,2,3,4,5});

List.add(6) // 예외발생

*asList()가 반환환 list의 크기를 변경불가 즉, 추가나 삭제가 불가능, 저장된 내용은 변경이 가능.

만약 크기를 변경하려할때는

List<Integer> list = new ArrayList<>(Arrays.asList(1,2,3,4,5));

 