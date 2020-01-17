# JavaScript

- https://developers.google.com/web/fundamentals/codelabs/push-notifications?hl=ko



service worker - push



- 푸시 메시지에 대한 사용자 구독 및 구독 취소 방법
- 수신 푸시 메시지 처리 방법
- 알림 표시 방법
- 알림 클릭에 대한 응답 방법



### 서비스워커 등록

- script/main.js

```javascript
if ('serviceWorker' in navigator && 'PushManager' in window) {
  console.log('Service Worker and Push is supported');

  navigator.serviceWorker.register('sw.js')
  .then(function(swReg) {
    console.log('Service Worker is registered', swReg);

    swRegistration = swReg;
  })
  .catch(function(error) {
    console.error('Service Worker Error', error);
  });
} else {
  console.warn('Push messaging is not supported');
  pushButton.textContent = 'Push Not Supported';
}
```



### 구독

```javascript
function initialiseUI() {
  pushButton.addEventListener('click', function() {
    pushButton.disabled = true;
    if (isSubscribed) {
      // TODO: Unsubscribe user
    } else {
      subscribeUser();
    }
  });

  // Set the initial subscription value
  swRegistration.pushManager.getSubscription()
  .then(function(subscription) {
    isSubscribed = !(subscription === null);

    updateSubscriptionOnServer(subscription);

    if (isSubscribed) {
      console.log('User IS subscribed.');
    } else {
      console.log('User is NOT subscribed.');
    }

    updateBtn();
  });
}

function updateBtn() {
  if (isSubscribed) {
    pushButton.textContent = 'Disable Push Messaging';
  } else {
    pushButton.textContent = 'Enable Push Messaging';
  }

  pushButton.disabled = false;
}
```



### 푸시리스너

- **self** 는 서비스 워커 자체를 참조하여 서비스 워커에 이벤트 리스너를 추가
- **showNotification()**  호출하여 알림 생성
- **event.waitUntil()** 프라미스를 취하여 브라우저는 전달된 프라미스가 확인될 때까지 
  서비스 워커를 활성화 및 실행 상태로 유지

```javascript
self.addEventListener('push', function(event) {
  console.log('[Service Worker] Push Received.');
  console.log(`[Service Worker] Push had this data: "${event.data.text()}"`);

  const title = 'Push Codelab';
  const options = {
    body: 'Yay it works.',
    icon: 'images/icon.png',
    badge: 'images/badge.png'
  };
  
 	// const notificationPromise = self.registration.showNotification(title, options);
	// event.waitUntil(notificationPromise);
  event.waitUntil(self.registration.showNotification(title, options));
});
```



### 푸시클릭

```javascript
self.addEventListener('notificationclick', function(event) {
  console.log('[Service Worker] Notification click Received.');

  event.notification.close();

  event.waitUntil(
    clients.openWindow('https://developers.google.com/web/')
  );
});
```



### 푸시보내기

- 보통 이를 위한 프로세스는 웹페이지에서 벡엔드로 구독을 전송하면 
  벡엔드가 구독에서 엔드포인트에 대한 API 호출을 실행하여 푸시 메시지를 트리거



### 구독취소

```javascript
pushButton.addEventListener('click', function() {
  pushButton.disabled = true;
  if (isSubscribed) {
    unsubscribeUser();
  } else {
    subscribeUser();
  }
});
```

