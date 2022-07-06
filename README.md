# ArrayBufferConverter

## convert between ArrayBuffer, Buffer, Uint8Array

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

## Uint8Array vs Base64 vs UnicodeString

```js
// @ts-check
'use strict';

// see https://coolaj86.com/articles/typedarray-buffer-to-base64-in-javascript/


/** @type {(buf:Uint8Array) => string} */
function bufferToBase64(buf) {
  var binstr = Array.prototype.map.call(buf, function (ch) {
    return String.fromCharCode(ch);
  }).join('');
  return btoa(binstr);
}

/** @type {(base64:string) => Uint8Array} */
function base64ToBuffer(base64) {
  var binstr = atob(base64);
  var buf = new Uint8Array(binstr.length);
  Array.prototype.forEach.call(binstr, function (ch, i) {
    buf[i] = ch.charCodeAt(0);
  });
  return buf;
}

// see https://coolaj86.com/articles/unicode-string-to-a-utf-8-typed-array-buffer-in-javascript/

/** 
 * string to uint array
 * @type {(s:string) => Uint8Array}
 */
function unicodeStringToTypedArray(s) {
  var escstr = encodeURIComponent(s);
  var binstr = escstr.replace(/%([0-9A-F]{2})/g, function (match, p1) {
    return String.fromCharCode('0x' + p1);
  });
  var ua = new Uint8Array(binstr.length);
  Array.prototype.forEach.call(binstr, function (ch, i) {
    ua[i] = ch.charCodeAt(0);
  });
  return ua;
}

/**
 * uint array to string
 * @type {(ua:Uint8Array) => string}
 */
function typedArrayToUnicodeString(ua) {
  /** @type {string} */
  var binstr = Array.prototype.map.call(ua, function (ch) {
    return String.fromCharCode(ch);
  }).join('');
  var escstr = binstr.replace(/(.)/g, function (m, p) {
    var code = p.charCodeAt(p).toString(16).toUpperCase();
    if (code.length < 2) {
      code = '0' + code;
    }
    return '%' + code;
  });
  return decodeURIComponent(escstr);
}

export {
  base64ToBuffer,
  bufferToBase64,
  unicodeStringToTypedArray,
  typedArrayToUnicodeString
};
```

## ArrayBuffer vs Base64

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
