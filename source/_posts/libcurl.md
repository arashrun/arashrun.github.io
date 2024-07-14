---
title: libcurl
date: 2023-07-14 09:54:53
categories:
  - 开源库
tags:
  - 网络
---

### before transfer

1. create handle
CURL *easy_handle = curl_easy_init();

2. set options
/* set URL to operate on */
res = curl_easy_setopt(easy_handle, CURLOPT_URL, "http://example.com/");

### transfering

3. begine transfer
// 3.1 simple transfer way
res = curl_easy_perform( easy_handle );
// 3.2 multi handle

// 3.3

### post transfer

> href: https://everything.curl.dev/libcurl
