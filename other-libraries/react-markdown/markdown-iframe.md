# react-markdown에 iframe tag 적용하기

기본적으로 markdown에는 iframe 태그를 적용할 수 없다.

iframe 영상을 markdown 내에 삽입하기 위해서는 별도의 dependency가 필요하다.&#x20;



rehype-raw와 rehype-sanitize 를 설치한다.&#x20;

```
npm install rehype-raw
npm install rehype-sanitize
```



ReactMarkdown을 import 해오는 부분에 설치한 플러그인을 가져온다.&#x20;

```javascript
import ReactMarkdown from 'react-markdown';
import rehypeRaw from 'rehype-raw';
import rehypeSanitize, { defaultSchema } from 'rehype-sanitize';
```



```javascript
<ReactMarkdown
  rehypePlugins={[
    rehypeRaw,
    rehypeSanitize({
      ...defaultSchema,
      attributes: {
        ...defaultSchema.attributes,
        iframe: [...(defaultSchema.attributes.iframe || [])],
      },
    }),
  ]}
>
  {content}
</ReactMarkdown>
```
