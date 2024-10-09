# Fails scanning my files: Error: EISDIR: illegal operation on a directory, read

에러 메시지 원문

```bash
internal/fs/utils.js:220
    throw err;
    ^

Error: EISDIR: illegal operation on a directory, read
    at Object.readSync (fs.js:506:3)
    at tryReadSync (fs.js:331:20)
    at Object.readFileSync (fs.js:368:19)
    at DestroyableTransform._transform (/Users/bjoern/projects/inventhora/frontend/node_modules/i18next-scanner/lib/index.js:26:34)
    at DestroyableTransform.Transform._read (/Users/bjoern/projects/inventhora/frontend/node_modules/i18next-scanner/node_modules/readable-stream/lib/_stream_transform.js:177:10)
    at DestroyableTransform.Transform._write (/Users/bjoern/projects/inventhora/frontend/node_modules/i18next-scanner/node_modules/readable-stream/lib/_stream_transform.js:164:83)
    at doWrite (/Users/bjoern/projects/inventhora/frontend/node_modules/i18next-scanner/node_modules/readable-stream/lib/_stream_writable.js:409:139)
    at writeOrBuffer (/Users/bjoern/projects/inventhora/frontend/node_modules/i18next-scanner/node_modules/readable-stream/lib/_stream_writable.js:398:5)
    at DestroyableTransform.Writable.write (/Users/bjoern/projects/inventhora/frontend/node_modules/i18next-scanner/node_modules/readable-stream/lib/_stream_writable.js:307:11)
    at DestroyableTransform.ondata (/Users/bjoern/projects/inventhora/frontend/node_modules/readable-stream/lib/_stream_readable.js:619:20) {
  errno: -21,
  syscall: 'read',
  code: 'EISDIR'
}
error Command failed with exit code 1.
```

요 에러 메시지는 파일을 읽어야하는데 폴더를 읽었다는 뜻. 혹시 폴더를 생성해놓기만 하고 파일을 생성하지 않은채로 까먹진 않았는지 확인해보자.
참고로 나는 i18next-scanner 명령어를 실행시키다가 해당 에러가 발생해서 알았다.
