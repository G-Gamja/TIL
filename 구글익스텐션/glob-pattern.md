# 글로브 패턴이란...

https://velog.io/@k7120792/Glob-%ED%8C%A8%ED%84%B4%EA%B3%BC-%EC%A0%95%EA%B7%9C%ED%91%9C%ED%98%84%EC%8B%9D

설명 

Glob properties follow a different, more flexible syntax than match patterns. Acceptable glob strings are URLs that may contain "wildcard" asterisks and question marks. The asterisk * matches any string of any length, including the empty string, while the question mark ? matches any single character.

For example, the glob `https://???.example.com/foo/*` matches any of the following:

`https://www.example.com/foo/bar`
`https://the.example.com/foo/`
However, it does not match the following:

`https://my.example.com/foo/bar`
`https://example.com/foo/`
`https://www.example.com/foo`

예시  1.  
This extension injects the content script into https://www.nytimes.com/arts/index.html and https://www.nytimes.com/jobs/index.html, but not into https://www.nytimes.com/sports/index.html:
```json
{
  "name": "My extension",
  ...
  "content_scripts": [
    {
      "matches": ["https://*.nytimes.com/*"],
      "include_globs": ["*nytimes.com/???s/*"],
      "js": ["contentScript.js"]
    }
  ],
  ...
}
```

```json
chrome.scripting.registerContentScript({
  id: 1,
  matches: ['https://*.nytimes.com/*'],
  include_globs: ['*nytimes.com/???s/*'],
  js: ['contentScript.js']
});
```

예시 2.

This extension injects the content script into https://history.nytimes.com and https://.nytimes.com/history, but not into https://science.nytimes.com or https://www.nytimes.com/science:
```json
{
  "name": "My extension",
  ...
  "content_scripts": [
    {
      "matches": ["https://*.nytimes.com/*"],
      "exclude_globs": ["*science*"],
      "js": ["contentScript.js"]
    }
  ],
  ...
}
```
```json
chrome.scripting.registerContentScript({
  id: 1,
  matches: ['https://*.nytimes.com/*'],
  exclude_globs: ['*science*'],
  js: ['contentScript.js']
});
```