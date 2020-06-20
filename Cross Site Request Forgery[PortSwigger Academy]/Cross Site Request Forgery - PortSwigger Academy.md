# Cross Site Request Forgery (CSRF)

First thing first, this is a guide from myself to myself about CSRF. This is the result of failing the interview on basic questions. I have not prepared fully for this and now, I am going to master it accordingly.

So what is CSRF?

CSRF is an attack in which the attacker abuses an already logged-in user by using its session and sending malicious commands on his behalf.

An example would be best to illustrate this, the following screenshots were taken from the academy of [portswigger.com](http://portswigger.com):

![Cross%20Site%20Request%20Forgery%20CSRF%2040441f5524d24373868c13677ea8fd25/Untitled.png](Cross%20Site%20Request%20Forgery%20CSRF%2040441f5524d24373868c13677ea8fd25/Untitled.png)

![Cross%20Site%20Request%20Forgery%20CSRF%2040441f5524d24373868c13677ea8fd25/Untitled%201.png](Cross%20Site%20Request%20Forgery%20CSRF%2040441f5524d24373868c13677ea8fd25/Untitled%201.png)

To login, I was given the following credentials: carlos / montoya.

![Cross%20Site%20Request%20Forgery%20CSRF%2040441f5524d24373868c13677ea8fd25/Untitled%202.png](Cross%20Site%20Request%20Forgery%20CSRF%2040441f5524d24373868c13677ea8fd25/Untitled%202.png)

I am logged in as carlos and have the ability to change the email.

![Cross%20Site%20Request%20Forgery%20CSRF%2040441f5524d24373868c13677ea8fd25/Untitled%203.png](Cross%20Site%20Request%20Forgery%20CSRF%2040441f5524d24373868c13677ea8fd25/Untitled%203.png)

![Cross%20Site%20Request%20Forgery%20CSRF%2040441f5524d24373868c13677ea8fd25/Untitled%204.png](Cross%20Site%20Request%20Forgery%20CSRF%2040441f5524d24373868c13677ea8fd25/Untitled%204.png)

It is seen here that the form submits itself to /email/change-email that is probably a function on a separate php page that changes a given email to the posted new value.

Notice that the input field's name is "email" and that it receives a value which should be an email address.

Moving over to the exploit server and to craft my exploit:

Here they present a cute interface for creating my exploit and then hosting it on a server & generating the url.

I begin by changing the "form action" attribute to 

![Cross%20Site%20Request%20Forgery%20CSRF%2040441f5524d24373868c13677ea8fd25/Untitled%205.png](Cross%20Site%20Request%20Forgery%20CSRF%2040441f5524d24373868c13677ea8fd25/Untitled%205.png)

I begin in changing the 'action' attribute to the function that changes the email ('email-change').

I craft a hidden input field that its name is the same as in the original site and add the new email.

Finally, I add a simple javascript code that simply submits all that data, without having a button present, making it execute the code behind the curtains. 

Pressing 'view exploit' runs the exploit as if carlos clicked the link. The generated malicious URL would be: "[https://ace21fdd1e7920fa80ed4c1f01060008.web-security-academy.net/exploit](https://ace21fdd1e7920fa80ed4c1f01060008.web-security-academy.net/exploit)"

When "carlos" "enters" the link, it executes the code above, the browser adds carlos's session because it is still valid for both of the sides since carlos is logged in and finally the email changes without carlos's concent and by an external source.

The CSRF attack described here is based on "POST" form submission.

A "GET" method is the easier one, which is simply changing the values of the parameters in the URL.

For example: 

[blog.com/account](http://blog.com/account)/change-password?password='attackerpass' 

This will abuse the 'password' parameter in "change-password" page, changing the password to 'attackerpass'.

This attack will work simply by sending this crafted malicious url to the victim, hoping he is logged into the system. 

If this works on someone, they should take a cyber awareness course, immediately.

# Mitigation

The best way to mitigate this attack is to use a CSRF token.

### CSRF Token

A CSRF token is a special crafted numeric string that is being generated on the server side and then sent to the web application. The token is being stored on the server for future matching and authentication.

The server sends the token to the web application and the token is then stored inside a hidden field somewhere on the page.

The use of CSRF tokens as an authenticating parameter is best explained as another layer of security in which a random value is added to one of the required parameters.

As we know, CSRF is executed by sending the exact parameters containing different values. This is to manipulate user data using an available user session.

If you add a random value, it is likely that the attacker won't be able to predict it. Moreover, the server on the other side is waiting for the generated token he assigned specifically for the request. 

The attacker has no way of knowing the CSRF token because each token is generated separately on every request. 

Example by steps:

1. Alice wants to connect to bank.com, her browser sends an HTTP request.
2. The server responds with an HTTP response that contains a generated token.
3. This token is temporarily saved on the server, waiting for Alice to enter her username and password.
4. Alice then enters her credentials and logs in. 
5. The server gives Alice a cookie containing its session ID, saves the token under Alice's session and uses it for future means of authentication whenever Alice performs manipulation on her data.

This prevents an attacker from executing a CSRF attack since he doesn't know Alice's token.

Unlike a regular attack which abuses only the session, which is provided by the browser automatically.

Example: 'Random User' receives 'token1' when she opens login.php.

The token is saved in a hidden field somewhere on the page.

Attacker receives 'token2'.

'Random User' logins as Alice and 'Alice' now gets a session ID that is stored inside a cookie. 'token1' is saved within Alice's session data on the server.

This way, each request will have to validate itself with the generated token, proving the authenticity of the user and eliminating the option of a forged request, since it will include a different token even if the session is identical. 

### Validation of CSRF token depends on request method

Some web applications correctly validate the authenticity of the token when using "POST" method, but skip this when a "GET" method is used. This way, an attacker can still execute the same CSRF attack from above by just changing the method value to "GET". Or sending the victim a crafted URL with the parameters containing the desired values.

### Validation of CSRF token depends on token being present

Some web applications correctly validate the authenticity of the token when it is present, but when it's not, then it skips validation. 

This is being executed by passing the email parameter AND the csrf parameter, but the csrf parameter should contain nothing.

### CSRF token is not tied to the user session

Some web applications don't validate that the csrf token is indeed connected to the specific user session. They validate the token if the token comes from a pool of pre-generated tokens on the server, without validating the session of the token. This results in attackers creating their own user, logging in, copying their own CSRF token and then execute the attack providing their own token which is legitimate because it does correlate with the server's pool of tokens.

### CSRF token is tied to a non-session cookie

I understood that there might be a situation where a web application ties the token to a different type of cookie, not the one who tracks the user, which can result in an exploitation of injecting a malicious cookie that contains the attacker's token into the victim's browser, allowing the attacker to execute commands as the user.

***Did not understand how to execute this attack on portswigger.***

wiener

sess: 1gPtR7onwIvWT5CNygds807664Zk78Ks

csrfKey: 7sZSr6sgiMd6SmU6QbOuxVtXN9JfTAJG

csrf: jikUC13QhYCbzU7zVTdW61STM0zWlVB5

Understood → Above are three keys. sessionid, csrfKey, and csrf. The csrfKey is the token used to be validated with the session. the normal csrf token is used with the POST method when changing the e-mail. This attack is a variation of the previous attack where the server generates a pool of accepted tokens. This means that even if I take someone else's tokens (attacker for example), I will be able to use them in order to bypass the csrf validation and execute commands as a different session ID. So I logged in with the attacker and wrote down the keys. Went to the exploit server and changed:

![Cross%20Site%20Request%20Forgery%20CSRF%2040441f5524d24373868c13677ea8fd25/Untitled%206.png](Cross%20Site%20Request%20Forgery%20CSRF%2040441f5524d24373868c13677ea8fd25/Untitled%206.png)

You can see that I've changed only the keys to my own keys. 'csrf' (wiener) = jikUC13QhYCbzU7zVTdW61STM0zWlVB5

'csrfKey' (wiener) = 7sZSr6sgiMd6SmU6QbOuxVtXN9JfTAJG

I noticed this while playing with burpsuite and sending different values. First I tried adding dummy data as the session ID. It failed. Meaning the association and validation is strong on the session ID.

What can I try more? 

I tried changing the 'csrfKey' to junk data and it failed as well. Showing there is yet a connection between the session and 'csrfKey' values. Also, validation of the token. PortSwigger wrote that this lab is a variation of the prev lab so I instantly knew the tokens won't matter as long as they come from the same server who generated them.

That's where I already went to the exploit server to create the img frame which executes malicious code that changes the 'csrfKey' value of the victim's session. This was due to a vulnerability in the search function that did some manipulation on the cookie after the search.

Screen:

![Cross%20Site%20Request%20Forgery%20CSRF%2040441f5524d24373868c13677ea8fd25/Untitled%207.png](Cross%20Site%20Request%20Forgery%20CSRF%2040441f5524d24373868c13677ea8fd25/Untitled%207.png)

'Set-Cookie: LastSearchTerm=abcd1234;' - If to do this righ, I can abuse the Set-Cookie to change the csrfKey to the attacker's csrfKey and then execute commands on behalf of the victim's session. 

This is done by running a search using the ?search parameter and to do this we need to fetch it with an img frame → <img src="https://vulnsite/?search=test%0d%0aSet-Cookie:%20csrfKey="token" onerror="documnet.forms[0].submit()" />

%0d - carriage return - return to the beggining of the line

%0a - newline - advance downward to the next line.

%20 - space.

![Cross%20Site%20Request%20Forgery%20CSRF%2040441f5524d24373868c13677ea8fd25/Untitled%208.png](Cross%20Site%20Request%20Forgery%20CSRF%2040441f5524d24373868c13677ea8fd25/Untitled%208.png)

This sets the attacker's csrfKey token inside the victim's session, basically injecting it to him after the victim clicked on the malicious link.

### CSRF token is simply duplicated in a cookie

**Did not quite understand this.**

Understood!

This attack is also a variation of the previous attack, but not quite. See, here the only resemblance  is the 'Set-Cookie:' attribute that I can abuse to set the csrf token to whatever I wish. 

Here the vulnerability resides in the security implementation of the token validation. The token validates every subsequent request by matching it to the token saved in the cookie, since the server does not save anything.

So the server generates a token and sends it to the web application.

The web application stores it inside the user cookie with the sessionID and everytime there is a POST request, it matches the sent token with the token inside the cookie.

In this lab, the 'Set-Cookie:' is vulnerable, just like in the previous example.

So we again change the csrf, but here to whatever we want, since it is going to check for the same value later when we will want to change the victim's email.

I change the csrf to 'fake-token' with the img frame that runs FIRST.

 In my malicious form, I add a hidden csrf field that submits also as 'fake-token'.

The img frame goes and changes the Set-Cookie attribute and sets csrf as 'fake-token'.

It results in an error and the malicious form submits itself with a request to go to the email change function with the csrf value set to 'fake-token' as well and with the desired email address.

The tokens authenticate and the email is changed.

![Cross%20Site%20Request%20Forgery%20CSRF%2040441f5524d24373868c13677ea8fd25/Untitled%209.png](Cross%20Site%20Request%20Forgery%20CSRF%2040441f5524d24373868c13677ea8fd25/Untitled%209.png)

### Validation of Referrer depends on header being present

Some web applications validate the referrer header to see where the request came from, but it is quickly bypassed by simply omitting the header. 

adding <meta name="referrer" content="never"> basically cancels the header in the request and allows CSRF.

sess: 5m9SGBaIMTxtx9TQWiWBT2p4GdLsoiKM

csrf: kjpTh1weJEZNdd1HVWwH5YnJNX3380A9

### CSRF with broken Referrer validation

This happens when a web application tries to prevent CSRF in a naive kind of way. The only check here is seeing if the url contains the domain name, if it does then it's ok, if not then it's a subsequent request the comes from a different origin hence - reject. 

This can be simply bypassed by adding the victim domain as a sub-domain to the attacker's main domain and the csrf will get executed since it will pass the validation.