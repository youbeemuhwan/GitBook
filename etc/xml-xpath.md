# XML 파싱 (Xpath)

XML 파일을 파싱해서 해당 내용을 화면에 띄우는 작업을 하면서 알게된 점 들을 정리해본다.



```
DocumentBuilderFactory factory = DocumentBuilderFactory.newInstance();
DocumentBuilder builder = factory.newDocumentBuilder();


Document document = builder.parse("xml/sample.xml");

// parse 안에는 xml 경로를 적어주면 된다.
```

Document 객체를 만들었다면 Node 인터페이스를 확장한   Document 인터페이스에 있는 기능들을 사용해서 파싱이 가능하다.&#x20;

Document 의 메서드는 아래 링크에서 확인하자.

[http://cris.joongbu.ac.kr/course/2019-1/jcp/api/org/w3c/dom/Document.html](http://cris.joongbu.ac.kr/course/2019-1/jcp/api/org/w3c/dom/Document.html)



또는 XPath 를 사용하는 방법도 있다.



<pre><code><strong>
</strong>XPath xPath = XPathFactory.newInstance().newXPath();

// xPath 객체 생성
<strong>
</strong><strong>XPathExpression xPathExpression = xPath.compile("form-data/*");
</strong><strong>
</strong><strong>// compile () 괄호 안에 파싱 표현식을 적어준다.
</strong><strong>
</strong>
Object evaluate1 = xPathExpression.evaluate(document, XPathConstants.NODESET);

// 표현식을 적용한 XPathExpression 객체의 evaluate 메서드를 사용한다.
// 첫번째 파라미터는 Document 객체, 두번째 파라미터는 반환타입을 적어준다.

반환타입은 NODESET, NODE, STRING 등등 몇가지 존재한다.
</code></pre>

NODESET 으로 반환값을 잡아도 타입은 Object 로 나오기에 NODELIST 타입으로 캐스팅 해준 뒤

for 문으로 원하는 값을 파싱했다.



XPath 에는 다양한 표현식 과 함수가 존재한다.  이것을 이용하면 기존 Document 메서드를 이용하는것보다 더 편하게 파싱할수있다.



Xpath 관련 문법은 아래 링크에서 확인하자

[https://www.w3schools.com/xml/xpath\_intro.asp](https://www.w3schools.com/xml/xpath\_intro.asp)
