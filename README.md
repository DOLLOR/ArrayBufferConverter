# ArrayBufferConverter
convert between ArrayBuffer, Buffer, Uint8Array

```ts
  // convert without copy
  const uint8Array2ArrayBuffer = (arr: Uint8Array) => arr.buffer;
  const arrayBuffer2Uint8Array = (ab: ArrayBuffer) => new Uint8Array(ab);
  const nodeBuffer2ArrayBuffer = (buffer: Buffer) => buffer.buffer;
  const arrayBuffer2NodeBuffer = (ab: ArrayBuffer) => Buffer.from(ab);
  // copy ArrayBuffer
  const cloneArrayBuffer = (ab: ArrayBuffer) => ab.slice(0);



  // example
  const u8 = Uint8Array.of(1, 2, 3, 4, 5, 6, 7, 8);
  console.log({ u8 });

  // TypedArray => ArrayBuffer
  const ab = uint8Array2ArrayBuffer(u8);

  // ArrayBuffer => Buffer
  const nodeBuffer = arrayBuffer2NodeBuffer(ab);

  // Buffer => ArrayBuffer => Uint8Array
  const u82 = arrayBuffer2Uint8Array(nodeBuffer2ArrayBuffer(nodeBuffer));
  u82.set([111, 111], 5);
  console.log({ u8 });

  // clone
  const u83 = arrayBuffer2Uint8Array(cloneArrayBuffer(ab));
  u83.set([222, 222], 5);
  console.log({ u8, u83 });
```

```js
// @ts-check

/**
 * @description https://www.isummation.com/blog/convert-arraybuffer-to-base64-string-and-vice-versa/
 */


/** @type {(buffer: ArrayBuffer) => string} */
function arrayBufferToBase64(buffer) {
  var binary = '';
  var bytes = new Uint8Array(buffer);
  var len = bytes.byteLength;
  for (var i = 0; i < len; i++) {
    binary += String.fromCharCode(bytes[i]);
  }
  return btoa(binary);
}

/** @type {(base64: string) => ArrayBuffer} */
function base64ToArrayBuffer(base64) {
  var binary_string = atob(base64);
  var len = binary_string.length;
  var bytes = new Uint8Array(len);
  for (var i = 0; i < len; i++) {
    bytes[i] = binary_string.charCodeAt(i);
  }
  return bytes.buffer;
}

const text = '测试';
const buffer = (new TextEncoder()).encode(text);
console.log(buffer);
const base64text = arrayBufferToBase64(buffer);
console.log(base64text);

```
