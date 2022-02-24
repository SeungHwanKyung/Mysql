
### select char_length(name) from tableb;  -- 문자열 길이를 알려줌

`SELECT CHAR_LENGTH("SQL Tutorial") AS LengthOfString;`


![스크린샷 2022-02-24 오전 9 59 15](https://user-images.githubusercontent.com/74452873/155436788-a5acbdb1-aaad-45b6-81e7-448c9e3d25f8.png)






### CONCAT 필요한 문자만 넣어서 출력. 

`SELECT CONCAT("SQL ", "Tutorial ", "is ", "fun!") AS ConcatenatedString;`

![스크린샷 2022-02-24 오전 10 00 28](https://user-images.githubusercontent.com/74452873/155436924-9a42a0b8-88f7-4298-9d50-3fdc435d2bb2.png)






### FIND_IN_SET --> String의 위치를 반환.
 
`SELECT FIND_IN_SET("q", "s,q,l");`

![스크린샷 2022-02-24 오전 10 01 25](https://user-images.githubusercontent.com/74452873/155437013-c673bc5c-f3b5-45ca-9bb8-348df4afed9c.png)






### INSERT --> 문자열 대치해주는 문법 같음.

~~~
INSERT("W3Schools.com", 1, 9, "Example")
Example.com
~~~


![스크린샷 2022-02-24 오전 10 01 51](https://user-images.githubusercontent.com/74452873/155437054-22536e22-b8e3-4d29-8b33-2493ec40c8cc.png)

 1 번째 스트링을 9 번까지 , 뒤에 있는 문자열로 대치


