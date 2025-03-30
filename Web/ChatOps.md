## ChatOps
> **Category:** web  

### Challenge Overview
We were given a deployed instance of a web app, and its source code. Its a PHP application, which uses LDAP.

### Code Review
Code appeared a bit complicated at first as I had never seen or heard about LDAP. I noticed that that in login.php, username was being sanitized for `*` but password wasn't. 

#### Approach
I found out that `*` can be used as an LDAP injection. Which meant I can skip the password check. Now I just needed the username. In setup.db, I saw `saic` and `club` being used. Tried username as `saic` and password as `*`, and I was in! The flag was visible in plain sight as soon as we log in.

**Flag:** `saic{ld4p_1s_4w3s0m3_4nd_1_l0v3_1t}`
