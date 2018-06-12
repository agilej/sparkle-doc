## Session Guidelines

Here are some general guidelines on sessions.

* **Do not store large objects in a session**. Instead you should store them in the database and save their id in the session. This will eliminate synchronization headaches and it won't fill up your session storage space \(depending on what session storage you chose, see below\). This will also be a good idea, if you modify the structure of an object and old versions of it are still in some user's cookies. With server-side session storages you can clear out the sessions, but with client-side storages, this is hard to mitigate.

* **Do not store critical data in session**. If the user clears their cookies or closes the browser, they will be lost. And with a client-side session storage, the user can read the data.

## Session API In Sparkle

`Session` interface is used to handle the session data, you can get it from `WebRequest#session`.

```java
Session session = request.session();

String uName = session.get("userName"); //read
session.set("uid", uid)                 //set
```

Another approach to get data from session is use `@SessionAttribute` on action method argument

```java
  //Sparkle framework will get session data with 'userName' and pass it as argument
  public String account(@SessionAttribute("userName") String userName){

  }
```

## Session Storage

Sparkle provides several storage mechanisms for the session data.

### Cookie Based Session Store

This is the default session stroage strategy Sparkle use.  CookieSessionStore saves the session data directly in a cookie on the client-side. The server retrieves the session data from the cookie and resemble it to use. That will greatly increase the horizonal scale ability of the application because you will get the SNA\(Share Nothing Architectur\) benifit from server side, but it is a controversial storage option and you have to think about the security implications of it:

* Cookies imply a strict size limit of 4kB. This is fine as you should not store large amounts of data in a session anyway, as described before. Storing the current user's database id in a session is usually ok.
* Security Concern. Some one may think it's not secure to store session data to cookie which can be seen on client side, even Sparkle encrypted the session data first.

Here is how Sparkle implement the cookie based session store. The session is encrypted before being stored in a cookie, This prevents the user from accessing and tampering the content of the cookie. Thus the session becomes a more secure place to store data. The encryption is done using a server-side secret key `secret_key_base` defined in `Config#setSecretKey`.

That means the security of cookie storage depends on this secret \(and on the digest algorithm, which defaults to SHA1, for compatibility\). So don't use a trivial secret, i.e. a word from a dictionary, or one which is shorter than 30 characters.

`secret_key_base` is used for specifying a key which allows sessions for the application to be verified against a known secure key to prevent tampering.

If you have found your application where the secret was exposed \(e.g. an application whose source was shared\), strongly consider changing the secret.

Another attention for cookie based session storage is the data serialization, defaultly Sparkle convert the session data to JSON format before encrypting, after receiving the data back it resemble the session from json data, because of the limitation of JSON's data type, you have to be caution of data type you used in session. For example if you set `Integer` or `Float` numbers to session, you can't directly get session data as expected data type and need to use `Number` instead. Here is one example:

```java
//set session

Integer uid = user.id();
session.set("uid", uid);

//get session, you should use Number instead

Number uidNumber = session.get("uid");
Integer uid = uidNumber.integerValue();
```

### Vendor provided session store

If you run Sparkle application under Servlet container like Tomcat, you may choose the vendor provided session store, pass this task of storing session data to underline servlet container, just like what you do in traditional servlet programming.

## Customize Session Store

Sparkle also provide ability to customize your own session store strategy if all the built-in strategies can't meet your requirement. For example you may want to use Redis to store session, in this scenery you can regiester your customized session store strategy to Sparkle, Here is the code demostration.

```java
class YourSessionStore implements SessionStore {

  public Object get(WebRequest request, String name) {
    //...
  }

  public set(WebRequest request, String name, Object value) {
    //...
  }

  public remove(WebRequest request, String name) {
    //...
  }

}

// register
```



