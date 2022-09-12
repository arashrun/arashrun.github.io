
<h3>before transfer</h3>
1. create handle
CURL *easy_handle = curl_easy_init();

2. set options
/* set URL to operate on */
res = curl_easy_setopt(easy_handle, CURLOPT_URL, "http://example.com/");

<h3>transfering</h3>
3. begine transfer
// 3.1 simple transfer way
res = curl_easy_perform( easy_handle );
// 3.2 multi handle

// 3.3

<h3>post transfer</h3>


> href: https://everything.curl.dev/libcurl
