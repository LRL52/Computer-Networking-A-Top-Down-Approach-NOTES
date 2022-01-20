> 注：问题翻译不准确的将使用原版英文代替

### HTTP1

1. 您的浏览器是否运行 HTTP 版本 1.0 或 1.1？服务器运行的是什么版本的 HTTP？

   服务器运行 HTTP1.1，我浏览器运行 HTTP1.1。

   ![image-20220120201515631](images/image-20220120201515631.png)

![image-20220120201534813](images/image-20220120201534813.png)

2.  您的浏览器指示哪些语言（如果有）可以接受服务器？

   我浏览器仅能接收简体中文，权重 0.9 

![image-20220120202348529](images/image-20220120202348529.png)

3. gaia.cs.umass.edu（也称为 wwwnet.cs.umass.edu）的互联网地址是什么？ 您的计算器的 Internet 地址是什么？

   我（挂了梯子）的IP是10.77.0.2

   gaia.cs.umass.edu 的 IP:128.119.245.12

![image-20220120202717847](images/image-20220120202717847.png)

4. 从服务器返回到浏览器的状态代码是什么？

   200 OK

5. 服务器上最后修改的 HTML 文件是什么时候？

![image-20220120203646391](images/image-20220120203646391.png)

6.  您的浏览器返回了多少 bytes 的内容？

   ![image-20220120203730278](images/image-20220120203730278.png)

7. By inspecting the raw data in the packet content window, do you see any headers within the data that are not displayed in the packet-listing window? If so, name one. 

   *Answer: no, I don’t see any in the HTTP Message below*

8. 检查从浏览器到服务器的第一个 HTTP GET 请求的内容，您是否在 HTTP GET 中看到“IF-MODIFIED-SINCE”行？

   ![image-20220120204308809](images/image-20220120204308809.png)

9. Inspect the contents of the server response. Did the server explicitly return the contents of the file? How can you tell?(检查服务器响应的内容。服务器是否显式返回文件的内容？你怎么知 道?)

   这句翻译很奇怪，我上网查了原文，并且研究了英文的参考资料， 这句话意思实际是结果中是否有你想得到内容。就是下图红色框圈的东 西。

![image-20220120205305582](images/image-20220120205305582.png)

10. 现在检查从浏览器到服务器的第二个 HTTP GET 请求的内容。您是否在 HTTP GET 中 看 到 “ IF-MODIFIED-SINCE ：” ？ 如 果 是 这 样 ， “IF-MODIFIED-SINCE：”标题后面的信息是什么？

![image-20220120205515988](images/image-20220120205515988.png)

![image-20220120211831781](images/image-20220120211831781.png)

意思是我们在请求时会发送给服务器上次它修改这个文件最后一次 时间，如果服务器发现仍然没有修改这个文件，就会返回 304 文件未改 变的信息（后面截图），调用本地 cache 缓存，如果改变就会重新得到。

11. What is the HTTP status code and phrase returned from the server in response to this second HTTP GET? Did the server explicitly return the contents of the file? Explain.

![image-20220120205716264](images/image-20220120205716264.png)





### HTTP2

1. How many HTTP GET request messages did your browser send? Which packet number in the trace contains the GET message for the Bill of Rights?（注：我把题目中的or改成了of）

   我的浏览器仅仅发送了一个 HTTP GET 请求信息，在追踪的数据包中，第48号包含请求GET message的`THE BILL OF RIGHTS`

![image-20220120212050784](images/image-20220120212050784.png)

2. 跟踪中的哪个 packet number 包含与 HTTP GET 请求响应相关联的状态代码(status code)和短语(phrase)？

   同上，还是第48个，状态码为200，短语是OK

3. 响应(response)中的状态代码(status code)和短语(phrase)是什么？ ANS:同第 2 题。

4. 需要多少个包含数据的 TCP 段来承载单个 HTTP 响应和权利法案的文本？

   ![image-20220120212702495](images/image-20220120212702495.png)

   从图中可以看到有4个

    

5. 您的浏览器发送了多少 HTTP GET 请求消息？ 这些 GET 请求发送到哪些 Internet 地 址？

   ![image-20220120213022648](images/image-20220120213022648.png)

   3条，发送到128.119.245.12 和 178.79.137.164

6. Can you tell whether your browser downloaded the two images serially, or whether they were downloaded from the two web sites in parallel? Explain. 

   ![image-20220120213627062](images/image-20220120213627062.png)

   我的图上是串行依次下载，但官方的解答是 并行

   *Answer: The downloads occurred in parallel. Note that the two GET messages for the images are in packets 17 and 20. The 200OK reply containing the images sow up as packets 25, and 54. Thus the request for the second image file (packet 20) was made BEFORE packet 25, the first image file was received.*

   我又重抓了一次包：

   ![image-20220120214035145](images/image-20220120214035145.png)

   这次用Edge，可以看到是并行下载的

7. 浏览器的初始 HTTP GET 消息，来自服务器的响应（状态代码 status code 和短语 phrase）是什么？

   服务器先返回 401 Unauthorized，即未授权，要求登陆才能访问。

   ![image-20220120214845419](images/image-20220120214845419.png)

8. 当您的浏览器第二次发送 HTTP GET 消息时，HTTP GET 消息中包含哪些新字段？

   如下图，包含了

   ​	`Authorization: Basic d2lyZXNoYXJrLXN0dWRlbnRzOm5ldHdvcms=`

   ![image-20220120215207214](images/image-20220120215207214.png)

![image-20220120215338821](images/image-20220120215338821.png)

​	PS:新版 Wireshark 自带给 base64 加密的用户名和密码解码，因此就不用网站工具了。

> You might want to first read up on HTTP authentication by reviewing the easy-to-read material on [“HTTP Access Authentication Framework”](http://frontier.userland.com/stories/storyReader$2159) 
>
> The username (wireshark-students) and password (network) that you entered are encoded in the string of characters (d2lyZXNoYXJrLXN0dWRlbnRzOm5ldHdvcms=) following  the “Authorization: Basic” header in the client’s HTTP GET message. While it may appear that your username and password are encrypted, they are simply encoded in a format known as Base64 format. The username and password are not encrypted! To see this, go to http://www.motobit.com/util/base64-decoder-encoder.asp and enter the base64-encoded string d2lyZXNoYXJrLXN0dWRlbnRz and decode. Voila! You have translated from Base64 encoding to ASCII encoding, and thus should see your username! To view the password, enter the remainder of the string Om5ldHdvcms= and press decode. Since anyone can download a tool like Wireshark and sniff packets (not just their own) passing by their network adaptor, and anyone can translate from Base64 to ASCII (you just did it!), it should be clear to you that simple passwords on WWW sites are not secure unless additional measures are taken.

![image-20220120215637611](images/image-20220120215637611.png)



###  实验体会

HTTP 实验总算搞完了，真累啊，效率贼低。。。
