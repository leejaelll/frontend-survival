# device type을 확인하는 함수

#### `checkOperatingSystem` &#x20;

👉🏻 navigator.userAgent로 기기를 확인한 후, 값을 리턴한다.&#x20;

```typescript
export const checkOperatingSystem = () => {
  const [os, setOs] = useState('Unknown');

  useEffect(() => {
    const detectOS = () => {
      const userAgent = navigator.userAgent;

      if (/Windows/i.test(userAgent)) {
        setOs('Windows');
      } else if (/Macintosh|MacIntel|Mac OS X/i.test(userAgent)) {
        setOs('macOS');
      } else if (/Linux/i.test(userAgent)) {
        setOs('Linux');
      } else if (/Android/i.test(userAgent)) {
        setOs('Android');
      } else if (/like Mac OS X/i.test(userAgent)) {
        setOs('iOS');
      } else {
        setOs('Unknown');
      }
    };

    detectOS();
  }, []);

  return os;
};
```



#### `useDeviceType`&#x20;

👉🏻 resize가 될 때마다 window.innerWidth를 체크한 후, 'Mobile', 'Tablet', 'Desktop'을 리턴

```typescript
const useDeviceType = () => {
  const [deviceType, setDeviceType] = useState('Desktop'); // 초기 값은 'Desktop'

  useEffect(() => {
    const handleResize = () => {
      const width = window.innerWidth;
      if (width <= 768) {
        setDeviceType('Mobile');
      } else if (width > 768 && width <= 1024) {
        setDeviceType('Tablet');
      } else {
        setDeviceType('Desktop');
      }
    };

    handleResize(); // 컴포넌트 마운트 시 실행

    window.addEventListener('resize', handleResize); // 창 크기 변경 감지

    return () => window.removeEventListener('resize', handleResize); // 클린업
  }, []);

  return deviceType;
};

export default useDeviceType;
```
