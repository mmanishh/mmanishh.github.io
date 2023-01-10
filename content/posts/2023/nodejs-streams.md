---
title: Understanding Node JS Streams by building Basic Video Streaming Server
date: 2023-01-09
image: /assets/images/2023/nodejs-streams.png
---

Stream on Node JS is one of the core modules that power NodeJS applications. They are a data-handling method and are used to read or write input into output sequentially. Streams are a way to handle reading/writing files, network communications, or any kind of end-to-end information exchange efficiently.

Streams are powerful because instead of loading all data in memory, streams read chunks of data piece by piece, processing its content without keeping it all in memory. This enables us to build powerful streaming applications like Netflix, and Youtube where you don’t have to wait to download that large file but start consuming once you get a continuous flow of chunk of data. 

If you are using Express and other frameworks, you are knowingly or unknowingly using streams because the stream is one of the core concepts of NodeJS. Everything that you use including Response, Request, File Reading, and Sockets are streams. Some of the examples where Node uses streams are:

- net.Socket is the main node API that is stream based, which underlies most of the following APIs
- process.stdin returns a stream connected to stdin
- process.stdout returns a stream connected to stdout
- process.stderr returns a stream connected to stderr
- fs.createReadStream() creates a readable stream to a file
- fs.createWriteStream() creates a writable stream to a file
- net.connect() initiates a stream-based connection
- http.request() returns an instance of the http.ClientRequest class, which is a writable stream
- zlib.createGzip() compress data using gzip (a compression algorithm) into a stream

## Why Streams?

Streams basically provide two major advantages compared to other data handling methods:

1. **Memory efficiency:** Since Streams loads file into memory chunk by chunk. It's efficient to load large data files by loading chunk by chunk rather than loading all in the memory.
2.  **Time efficiency:** Once you have the data file chunk by chunk, you can process the chunk of data as soon as you have the data instead of waiting to get the whole of the data.

## Types of Streams

There are 4 types of streams in Node.js:

- Writable: streams to which we can write data. For example, fs.createWriteStream() lets us write data to a file using streams.
- Readable: streams from which data can be read. For example: fs.createReadStream() lets us read the contents of a file.
- Duplex: streams that are both Readable and Writable. For example, net.Socket
- Transform: streams that can modify or transform the data as it is written and read. For example, in the instance of file compression, you can write compressed data and read decompressed data to and from a file.

## Create a Video Streaming Server with NodeJS Streams

Okay, Let's start using the stream in real life. We are going to create a video streaming app which basically streams a video rather than loading all of its content. We have a GitHub repo to get started where the code and Express app is bootstrapped through express-generator [here](https://github.com/mmanishh/nodejs-stream) 

We have a sample video file which is approximately 30 MB which will be used to stream. Let's say you have a very large file of 1 GB or more, streaming helps to stream the file chunk by chunk and we can choose the chunk size. At the demo code, We have used CHUNK_SIZE to approximately 1 MB.

![Gitpod Repo](/assets/images/2023/nodejs-streams.png)

You can get started by cloning the repo and installing the necessary dependencies.

There is only one route where we stream the video on the index route at app.js.

```js
app.get('/', function (req, res) {
    const range = req.headers.range || '0';

    const videoSize = fs.statSync(videoPath).size; // 31491130
    const CHUNK_SIZE = 10 ** 6; // 1000000 ~ 1MB

    const start = Number(range.replace(/\D/g, ''));
    const end = Math.min(start + CHUNK_SIZE, videoSize - 1);

    // headers
    const contentLength = end - start + 1;
    const headers = {
        'Content-Range': `bytes ${start}-${end}/${videoSize}`,
        'Accept-Ranges': 'bytes',
        'Content-Length': contentLength,
        'Content-Type': 'video/mp4',
    };

    // HTTP Status 206 for Partial Content
    res.writeHead(206, headers);
    // create video read stream for this particular chunk
    const videoStream = fs.createReadStream(videoPath, { start, end });

    // Stream the video chunk to the client
    videoStream.pipe(res);
});
```

Here, I am trying to explain line by line in the context of variables:

- range:  is the start of the file byte from where the server streams the video, We initialized it with 0 because initially, we are starting from zero but once we stream a chunk of file the range will be the end of the first chunk of file.
- videoSize : It is the total size of video which is 31491130 ~ 30 MB
- CHUNK_SIZE: It is the size of the chunk which are sent to client. 10^6 ~ 1MB
- start: It is the start of chunk
- end: It is the end of the chunk
- contentLength: It is the length of the chunk which is calculated by end-start+1
- totalChunk: It is not defined though but it is total number of chunk that will be served to client which can be calculated by videoSize/CHUNK_SIZE ~ 31.49. So, approximately 32 chunk of file will be send over to client instead of one whole chunk.
- headers : Object which is header stating meta info about the response.
- videoStream: We created a readable stream out of the video file that we have, indicating where to start and end the file with options: start and end. It will read a chunk from start to end. Then We piped the readable stream to Response which is a Writable stream and streamed it to the client

## Conclusion

We went through what streams in NodeJS and what makes NodeJS powerful. We went through a different example of streams in the NodeJS module itself also we learn about different types of streams and why the stream is an efficient way of data handling. Also, we build a basic video streaming server based on NodeJS. I hope you got a clear understanding of NodeJS Streams and their application.





