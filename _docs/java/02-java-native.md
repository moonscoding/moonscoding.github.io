---

title: "Java Native"
permalink: /docs/native/

toc: true
toc_sticky: true
toc_label: "My Table of Contents"
toc_icon: "cog"

---







## Primitive

- Primitive?
  - 원시 자료형이란 뜻으로 과학에서 프로그래밍 언어가 제공하는 자료형 중 하나. 내장형 혹은 기본형으로도 불림 
  - [wiki]([https://ko.wikipedia.org/wiki/%EC%9B%90%EC%8B%9C_%EC%9E%90%EB%A3%8C%ED%98%95](https://ko.wikipedia.org/wiki/원시_자료형))
- Java Primitive Type
  - byte, char, short, int, long, float, double, boolean



### int casting

| FROM | TO      | METHOD                                                       | EX.  |
| ---- | ------- | ------------------------------------------------------------ | ---- |
| int  | byte    | -                                                            |      |
| int  | char    | char c = (char) `int`;                                       |      |
| int  | short   | -                                                            |      |
| int  | long    | long l = `int`;<br />Long l = new Long(`int`);<br />Long l = Long.valueOf(`int`); |      |
| int  | float   | -                                                            |      |
| int  | double  | double d = `int`;<br />Double d = new Double(`int `); <br />Double d = Double.valueOf(`int`); |      |
| int  | boolean | -                                                            |      |
| int  | string  | String s = String.valueOf(`int`);<br />String s = Integer.toString(`int`); |      |



### long casting

| FROM | TO      | METHOD                                                       | EX.  |
| ---- | ------- | ------------------------------------------------------------ | ---- |
| long | byte    | -                                                            |      |
| long | char    | -                                                            |      |
| long | short   | -                                                            |      |
| long | int     | int i = (int) `long`;<br />int i = `wrapperLong`.intValue(); |      |
| long | float   | -                                                            |      |
| long | double  | -                                                            |      |
| long | boolean | -                                                            |      |
| long | string  | String s = String.valueOf(`long`);<br />String s = Long.toString(`long`); |      |



### float casting

| FROM  | TO      | METHOD | EX.  |
| ----- | ------- | ------ | ---- |
| float | byte    | -      |      |
| float | char    | -      |      |
| float | short   | -      |      |
| float | int     | -      |      |
| float | long    | -      |      |
| float | double  | -      |      |
| float | boolean | -      |      |
| float | string  | -      |      |



### double casting

| FROM   | TO      | METHOD | EX.  |
| ------ | ------- | ------ | ---- |
| double | byte    | -      |      |
| double | char    | -      |      |
| double | short   | -      |      |
| double | int     | -      |      |
| double | long    | -      |      |
| double | float   | -      |      |
| double | boolean | -      |      |
| double | string  | -      |      |



### boolean casting

| FROM    | TO     | METHOD | EX.  |
| ------- | ------ | ------ | ---- |
| boolean | byte   | -      |      |
| boolean | char   | -      |      |
| boolean | short  | -      |      |
| boolean | int    | -      |      |
| boolean | long   | -      |      |
| boolean | float  | -      |      |
| boolean | long   | -      |      |
| boolean | string | -      |      |



## String

### StringPool



### StringBuilder & StringBuffer



## Enum



## Object

### Objects



## Math



## Date



## Format



## Regex

정규식(Regular Expression)

