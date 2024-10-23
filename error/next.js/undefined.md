# 외부 이미지 적용하기

{% hint style="danger" %}
Error: Invalid src prop (https://disarli-storage.s3.us-east-2.amazonaws.com/user/clzauirgl0000ssslsg69sexf/chatbots/2gc5nvdMhZRoH4kxABcmw.jpeg) on `next/image`, hostname "disarli-storage.s3.us-east-2.amazonaws.com" is not configured under images in your `next.config.js`
{% endhint %}

👉🏻 Next.js에서 외부 이미지를 로드하려고 할 때 발생하는 오류



호스트를 명시적으로 명시해줘야한다.&#x20;

{% code title="next.confing.mjs" %}
```js
images: {
  remotePatterns: [
   { protocol: "https", hostname: "disarli-storage.s3.us-east-2.amazonaws.com" }, 
   { protocol: "https", hostname: "disarli-web-storage.s3.ap-northeast-2.amazonaws.com" },
  ],
},
```
{% endcode %}
