## Lombok



### @ToString

toString() 메소드를 Override 하여 각각 필드 (non-static) 를 출력할 수 있도록 처리

 

`includeFieldNames`

- 필드 이름 생략
- includeFieldNames = true, includeFieldNames = false



`exclude`

- 특정 필드 생략
- exclude = {"field1", "field2"}



`of`

- 명시적으로 특정 필드를 포함하게
- of = {"field1", "field2"}



> parent class의 instance member를 출력할 수 있는가 ?

`callSuper`

- 부모의 옵션을 호출합니다.
- callSuper = true, callSueper = false



> static member를 출력할 수 있는가?

...



https://projectlombok.org/features/ToString