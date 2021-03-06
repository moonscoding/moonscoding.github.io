# JavaScript

- https://developers.google.com/web/fundamentals/primers/promises?hl=ko

- [https://joshua1988.github.io/web-development/javascript/promise-for-beginners/#promise가-뭔가요](https://joshua1988.github.io/web-development/javascript/promise-for-beginners/#promise가-뭔가요)

## Promise



프로미스는 주로 서버에서 받아온 데이터를 화면에 표시할 때 사용



### state

- Pending (대기)
  - 비동기 처리 로직이 아직 완료되지 않은
- Fulfilled (이행)
  - 비동기 처리가 완료되어 프로미스가 결과 값을 반환
- Rejected (실패)
  - 비동기 처리가 실패하거나 오류가 발생



Pending

```javascript
new Promise();
```

```javascript
new Promise(function(resolve, reject) {
  
});
```



Fulfilled

resolve() 함수가 실행되면 Fulfilled 상태가 됩니다.

```javascript
new Promise(function(resolve, reject) {
	resolve();
});
```

```javascript
function getData() {
  return new Promise(function (resolve, reject) {
    var data = 100;
    resolve(data);
  });
}

getData().then(function (resolveData) {
  console.log(resolveData); // 100
});
```



Rejected

reject() 함수가 실행되면 Rejected 상태가 됩니다.

```javascript
new Promise(function(resolve, reject) {
	reject();
});
```

```javascript
function getData() {
  return new Promise(function(resolve, reject) {
    reject(new Error("request is failed"));
  })
}

getData().then().catch(function(err) {
  console.log(err);
});
```



### Chaining

then의 callback 함수로 들어가는 인자 함수의 반환값이 promise를 반환하지 않아도

chaining 연결이 가능하다.

```javascript
new Promise(function(resolve, reject){
  setTimeout(function() {
    resolve(1);
  }, 2000);
})
.then(function(result) {
  console.log(result); // 1
  return result + 10;
})
.then(function(result) {
  console.log(result); // 11
  return result + 20;
})
.then(function(result) {
  console.log(result); // 31
});
```



### Exception Handling



1. then() second param

```javascript
getData().then(
	handleSuccess,
	handleError
);
```



2. catch()

```javascript
getData().then().catch();
```



> 가급적 catch() 사용 권장

더 많은 예외 상황의 바운더리를 가지고 있음

