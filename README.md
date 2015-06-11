# 마크다운 핵심 문법
문서작성이 헷갈리기 쉬우므로 잘 알아두자.  
[참고사이트](http://goo.gl/Tp0TfH)


~~~
제목은 6단계 #갯수로 확인

강조는 *
BOLD는 **
두개 조합도 가능
-
링크
인라인 링크
[Google](http://www.google.com/ "이건구글이지요"). 
툴팁을 표현수 도 있다

참조 링크
[Google] [1].
[1]: http://www.google.com/

또다른읽기 편한링크
[Google]
[GOogle]: http://www.googl.com

링크정의 구문은 유니크해야 하며 대소문자 구분이 없다
링크정의 구문이 귀찮다. 그럼 <>안에 URL만 넣어주면 인식

그림
링크와 동일한 구문. 단지 주소만이 다를뿐.

수평선은 -,*,_를 세개이상 나열

인용
기본적으로 >를 앞에 넣어둠
인용안에 인용은 >넣고 공백넣고 다시 >
다중중첩 가능

리스트
-,+,*를 맨 앞에 적는다.

숫자로 인용하고 싶을때는 숫자+마침표
자신이 적은 숫자와 상관없이 순서대로 번호가 매겨짐

2개의 리스트를 나열하고 싶다면 리스트 사이에 공백

리스트 안에 리스트
인덴트용 스페이스 4개
스페이스 8개면 다시 한단계 아래
리스트하나 마다 다른 내용을 넣어도 잘 인식함. 엔터필요. 리스트 안에 인용할때도 마찬가지로 엔터
코드도 역시 엔터치고 인덴트 4개

마크다운에서 표현하지 못하는 표현양식은 인라인 HTML로 가능 
<strike>
HTML 문법안에 마크다운 문법도 가능

소스코드는 인덴트용 스페이스 4개
코드 블럭내에는 마크다운이나 HTML을 사용금지.
간단한 라인코드는 ``로 감싼다.
~~~
