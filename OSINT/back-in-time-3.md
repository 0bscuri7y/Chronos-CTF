## Back In Time - 3

### Description

A secret folder has been left behind on ex-SAIC member's GitHub that was never meant to be found. But what’s locked inside remains a mystery.

Rumors suggest that the key to unlocking it is hidden somewhere on the web, buried within paragraphs...

Your mission is to track down the password and reveal what lies within.

---

### Solution

From the github repo found in back-in-time-2, we see the is a special repository for back-in-time-3. We find a zip file and something.md which points to spider man wikipedia page. 

From the commits we can se FLAG.pff was deleted. It contained - 

```
ga2dmnatdqy5k0yu95932062305202
f544631f0af6e235d86b09e869d621620820820db978816b66b501855f09e9db
```

Second line looked like a hash, which i threw into crackstation.net website. It told me its a partial hash match for `Straczynski`. I tried extracting the zip file with this `Straczynski` as password, and it worked. Flag was in a text file inside.


Flag: `saic{c3wl_w0rd5_f0r_bru73f0rc1ng!!}`

