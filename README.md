![utfx - A compact library to process, convert, encode and decode UTF8 / UTF16 in JavaScript.](https://raw.github.com/dcodeIO/utfx/master/utfx.png)
====
**utfx** is a compact library to process, convert, encode and decode UTF8 / UTF16 in JavaScript using arbitrary sources
and destinations through the use of callbacks.

As a side product it also provides polyfills for `String.fromCodePoint` and `String#codePointAt`.

### Background

While there are already tons of UTF8 libraries around, most of them are based on a specific data scheme (e.g. binary
strings) that may [not be appropriate in specific use cases](https://github.com/dcodeIO/utfx/wiki#faq). To work around
this, utfx provides the developer with the freedom to implement the low level operations (obtaining and outputting data)
on their own.

API
---

### Class [TruncatedError](doco/TruncatedError.md)

An error indicating a truncated source. Contains the remaining bytes as an array in its `bytes` property.

### encodeUTF8(src, dst)

Encodes UTF8 code points to an arbitrary output destination of UTF8 bytes.

| Parameter       | Type            | Description
|-----------------|-----------------|---------------
| src             | *function():(number &#124; null) &#124; !Array.&lt;number&gt; &#124; number* | Code points source, either as a function returning the next code point respectively `null` if there are no more code points left, an array of code points or a single numeric code point. 
| dst             | *function(number) &#124; Array.&lt;number&gt; &#124; undefined* | Bytes destination, either as a function successively called with the next byte, an array to be filled with the encoded bytes or omitted to make this function return a binary string. 
| **@returns**    | *undefined &#124; string* | A binary string if `dst` has been omitted, otherwise `undefined` 
| **@throws**     | *TypeError*     | If arguments are invalid 
| **@throws**     | *RangeError*    | If a code point is invalid in UTF8 

### decodeUTF8(src, dst)

Decodes an arbitrary input source of UTF8 bytes to UTF8 code points.

| Parameter       | Type            | Description
|-----------------|-----------------|---------------
| src             | *function():(number &#124; null) &#124; !Array.&lt;number&gt; &#124; string* | Bytes source, either as a function returning the next byte respectively `null` if there are no more bytes left, an array of bytes or a binary string. 
| dst             | *function(number) &#124; !Array.&lt;number&gt;* | Code points destination, either as a function successively called with each decoded code point or an array to be filled with the decoded code points. 
| **@throws**     | *TypeError*     | If arguments are invalid 
| **@throws**     | *RangeError*    | If a starting byte is invalid in UTF8 
| **@throws**     | *utfx.TruncatedError* | If the last sequence is truncated. Has an array property `bytes` holding the remaining bytes. 

### UTF16toUTF8(src, dst)

Converts an arbitrary input source of UTF16 characters to an arbitrary output destination of UTF8 code points.

| Parameter       | Type            | Description
|-----------------|-----------------|---------------
| src             | *function():(number &#124; null) &#124; !Array.&lt;number&gt; &#124; string* | Characters source, either as a function returning the next char code respectively `null` if there are no more characters left, an array of char codes or a standard JavaScript string. 
| dst             | *function(number) &#124; Array.&lt;number&gt;* | Code points destination, either as a function successively called with each converted code point or an array to be filled with the converted code points. 
| **@throws**     | *TypeError*     | If arguments are invalid or a char code is invalid 
| **@throws**     | *RangeError*    | If a char code is out of range 

### UTF8toUTF16(src, dst)

Converts an arbitrary input source of UTF8 code points to an arbitrary output destination of UTF16 characters.

| Parameter       | Type            | Description
|-----------------|-----------------|---------------
| src             | *function():(number &#124; null) &#124; !Array.&lt;number&gt; &#124; number* | Code points source, either as a function returning the next code point respectively `null` if there are no more code points left, an array of code points or a single numeric code point. 
| dst             | *function(number) &#124; !Array.&lt;number&gt; &#124; undefined* | Characters destination, either as a function successively called with each converted char code, an array to be filled with the converted char codes or omitted to make this function return a standard JavaScript string. 
| **@returns**    | *undefined &#124; string* | A standard JavaScript string if `dst` has been omitted, otherwise `undefined` 
| **@throws**     | *TypeError*     | If arguments are invalid or a code point is invalid 
| **@throws**     | *RangeError*    | If a code point is out of range 

### encodeUTF16toUTF8(src, dst)

Converts and encodes an arbitrary input source of UTF16 characters to an arbitrary output destination of UTF8
bytes.

| Parameter       | Type            | Description
|-----------------|-----------------|---------------
| src             | *function():(number &#124; null) &#124; !Array.&lt;number&gt; &#124; string* | Characters source, either as a function returning the next char code respectively `null` if there are no more characters left, an array of char codes or a standard JavaScript string. 
| dst             | *function(number) &#124; Array.&lt;number&gt; &#124; undefined* | Bytes destination, either as a function successively called with the next byte, an array to be filled with the encoded bytes or omitted to make this function return a binary string. 
| **@returns**    | *undefined &#124; string* | A binary string if `dst` has been omitted, otherwise `undefined` 
| **@throws**     | *TypeError*     | If arguments are invalid or a char code is invalid 
| **@throws**     | *RangeError*    | If a char code is out of range 

### decodeUTF8toUTF16(src, dst)

Decodes and converts an arbitrary input source of UTF8 bytes to an arbitrary output destination of UTF16
characters.

| Parameter       | Type            | Description
|-----------------|-----------------|---------------
| src             | *function():(number &#124; null) &#124; !Array.&lt;number&gt; &#124; string* | Bytes source, either as a function returning the next byte respectively `null` if there are no more bytes left, an array of bytes or a binary string. 
| dst             | *function(number) &#124; !Array.&lt;number&gt; &#124; undefined* | Characters destination, either as a function successively called with each converted char code, an array to be filled with the converted char codes or omitted to make this function return a standard JavaScript string. 
| **@returns**    | *undefined &#124; string* | A standard JavaScript string if `dst` has been omitted, otherwise `undefined` 
| **@throws**     | *TypeError*     | If arguments are invalid 
| **@throws**     | *RangeError*    | If a starting byte is invalid in UTF8 
| **@throws**     | *utfx.TruncatedError* | If the last sequence is truncated. Has an array property `bytes` holding the remaining bytes. 

### calculateUTF8(src)

Calculates the number of UTF8 bytes required to store an arbitrary input source of UTF8 code points.

| Parameter       | Type            | Description
|-----------------|-----------------|---------------
| src             | *function():(number &#124; null) &#124; Array.&lt;number&gt; &#124; number* | Code points source, either as a function returning the next code point respectively `null` if there are no more code points left, an array of code points or a single numeric code point. 
| **@returns**    | *number*        | Number of UTF8 bytes required 
| **@throws**     | *TypeError*     | If arguments are invalid 
| **@throws**     | *RangeError*    | If a code point is out of range 

### calculateUTF16asUTF8(src)

Calculates the number of UTF8 bytes required to store an arbitrary input source of UTF16 characters when
converted to UTF8 code points.

| Parameter       | Type            | Description
|-----------------|-----------------|---------------
| src             | *function():(number &#124; null) &#124; !Array.&lt;number&gt; &#124; string* | Characters source, either as a function returning the next char code respectively `null` if there are no more characters left, an array of char codes or a standard JavaScript string. 
| **@returns**    | *number*        | Number of UTF8 bytes required 
| **@throws**     | *TypeError*     | If arguments are invalid 
| **@throws**     | *RangeError*    | If an intermediate code point is out of range 

### fromCodePoint(var_args)

A polyfill for `String.fromCodePoint`.

| Parameter       | Type            | Description
|-----------------|-----------------|---------------
| var_args        | *...number*     | Code points 
| **@returns**    | *string*        | Standard JavaScript string 
| **@throws**     | *TypeError*     | If arguments are invalid or a code point is invalid 
| **@throws**     | *RangeError*    | If a code point is out of range 

### codePointAt(s, i)

A polyfill for `String#codePointAt`.

| Parameter       | Type            | Description
|-----------------|-----------------|---------------
| s               | *string*        | Standard JavaScript string 
| i               | *number*        | Index 
| **@returns**    | *number &#124; undefined* | Code point or `undefined` if `i` is out of range 
| **@throws**     | *TypeError*     | If arguments are invalid 

### polyfill(override=)

Installs utfx as a polyfill for `String.fromCodePoint` and `String#codePointAt` if not implemented.

| Parameter       | Type            | Description
|-----------------|-----------------|---------------
| override        | *boolean*       | Overrides an existing implementation if `true`, defaults to `false` 
| **@returns**    | *!Object.&lt;string,*&gt;* | utfx namespace 

Usage
-----
* **node.js**: `npm install utfx`   
   
   ```js
   var utfx = require("utfx");
   ...
   ```

* **Browser**: `<script src="/path/to/utfx.min.js"></script>`   
   
   ```js
   var utfx = dcodeIO.utfx;
   ...
   ```
   
* **Require.js/AMD**   
   
   ```js
   require.config({
       "paths": {
           "utfx": "/path/to/utfx.min.js"
       }
   });
   require(["utfx"], function(utfx) {
       ...
   }
   ```

Downloads
---------
* [Distributions](https://github.com/dcodeIO/utfx/tree/master/dist)

FAQ and examples
----------------
* [Wiki](https://github.com/dcodeIO/utfx/wiki)

License
-------
Apache License, Version 2.0
