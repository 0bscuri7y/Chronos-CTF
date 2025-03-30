## Star Wars 


# Description:

```
Don't search blindly. Look carefully and you will find what you are looking for.

Hint! This challenge is specially made for blind persons :P
``` 

We've been provided with a jpg image. 

![Original image](images/star_wars.jpg)

I immediately check the exif, and run stegseek on every JPGs. Upon using stegseek with rockyou.txt, we successfully extacted some data.  

The data happened to be of the form base64. Upon decoding it, we get the string `becomeajedimasteryouwill`.
