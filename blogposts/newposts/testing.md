# Hello!
# `test`
## `test again`
```javascript
import Link from 'next/link';
import { useRouter } from 'next/router';
import ReactMarkdown from 'react-markdown';
import remarkGfm from 'remark-gfm';
import styled from 'styled-components';
import useSWR from 'swr';
import {Prism as SyntaxHighlighter} from 'react-syntax-highlighter';
import style from 'react-syntax-highlighter/dist/cjs/styles/prism/vsc-dark-plus';
const fetcher = (...args: any) => fetch(args).then(async res => {
  return await res.text()
});
const StyledLink = styled.a`
  color: #428af5;
`
const Post = () => { 
  const router = useRouter()
  const { id } = router.query;
    const { data, error } = useSWR(id ? `https://raw.githubusercontent.com/Squirrelcoding/SoftsquirrelProps/main/blogposts/newposts/${id}.md` : null, fetcher);
    if (error) return <div>error: {JSON.stringify(error)}</div>
    if (!data) return <div>loading...</div>
    console.log(data);

    return (
      <>
        <ReactMarkdown remarkPlugins={[remarkGfm]} 
            components={{
              code({node, inline, className, children, ...props}) {
                const match = /language-(\w+)/.exec(className || '')!;
                console.log(match);
                return !inline && match ? (
                  <SyntaxHighlighter
                    language={match[1]}
                    preTag="code"
                    style={style}
                    showLineNumbers  
                    {...props}
                  >
                    {String(children).replace(/\n$/, '')}
                  </SyntaxHighlighter>
                ) : (
                  <code className={className} {...props}>
                    {children}
                  </code>
                )
              }
            }}
        >{data}</ReactMarkdown>
        <br/>
        <Link href="/updates" passHref>
            <StyledLink>See more posts</StyledLink>
        </Link>
      </>

    )
}

export default Post
```

> This is a blockquote

*Italic*

**bold**
