# What is HTTP and how it works?
Http developed by [Tim Berners-Lee](https://en.wikipedia.org/wiki/Tim_Berners-Lee) when he was in CERN year 1989-1991.

Everyone must have seen this word, when you are using browser to open a web page. The very first word http (hyper text transfer protocol), everytime you use when browser sends request and recieve responces on internet, more generalized sentence when you open a web page. Http propotocal defines how information going to be formatted and transmitted over the network.

When user click on the link, or type something on browser address bar, http sends request to server and server evaluates the request and return responce back to client (in this case client is browser). Http works based on request/response pair. It means each request to the server get a response along with header and content. Header can contains many information such as type of server, date time of request and many more.

HTTP (**H**yper **T**ext **T**ransfer **P**rotocol) - If you seprate Transfer word from here you get two words that need to understand. **Protocol** which means set to rules which tells how the two different entities (computers) communicates with each other and **Hypertext** is simply a text which refers or redirect to another text or web document. That might be more that just hypertext in today's world but take this as is defined back in the days.

The developers of the HTTP make sure that this is as simple as where everyone can understand easily and in plain text. User doesn't need to know the internal mechanics, how it works but it's request and responses are easily readable. For example it used following keywords called verbs to send request to other entity (server or computer).
| VERB | DESCRIPTION |
|:------|:-------------|
| **GET** | The GET method requests a representation of the specified resource. Requests using GET should only retrieve data. |
| **POST**| The POST method submits an entity to the specified resource, often causing a change in state or side effects on the server.|
| **PUT**| The PUT method replaces all current representations of the target resource with the request payload.|
| **PATCH**| The PATCH method applies partial modifications to a resource.|
| **DELETE**| The DELETE method deletes the specified resource.|
| **CONNECT**| The CONNECT method establishes a tunnel to the server identified by the target resource.|
| **HEAD**| The HEAD method asks for a response identical to a GET request, but without the response body.|
| **OPTIONS**| The OPTIONS method describes the communication options for the target resource.|
| **TRACE**| The TRACE method performs a message loop-back test along the path to the target resource.|
    
## Http is stateless
Http protocol is stateless in nature. It means you cannot persist any value between the transaction. In other words, one request has no memory of other requests. Each request is unique in nature and separately generated from each other. Requests are not connected to each other by any mean. But why is that? Http is designed not to fall in situation of request sequencing which makes this protocol simple and easy to use.

### Solution
Sessions, that stored the states between client (browser) and server. When the web page loads it sends the information back and forth and store the information temporarily in form of cookies. When http pass information back-n-forth and uses header to transmit information. 

## Versions
Like most of the technologies in the today's world, evolved significantly over the time. Some features are discontinued, absolete and new features being added. Same with http. 

HTTP uses a “<major>.<minor>” numbering scheme to indicate versions of the protocol. The protocol versioning policy is intended to allow the sender to indicate the format of a message and its capacity for understanding further HTTP communication, rather than the features obtained via that communication

| Version | DESCRIPTION |
|:------|:-------------|
|Http/0.9| Initially there was no version number. This was given later point just to differentiate from other version. This is very simple and one line protocol. Get is the only method provided at the time which has resource url with any server, protocol infomation. In this version, there was no headers available which means only one format can be transmitted which is HTML (Hyper Text Transfer Protocol)|
|Http 1.0| Couple of features added to this. Added version information, status code to tell if request was successfully processed or not, introduce header concept. Now possible to send different content type other than html. |
|Http 1.1| |
|Http 2 | |
|Http 3 | |



