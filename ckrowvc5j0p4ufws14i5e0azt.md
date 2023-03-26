---
title: "Preventing CSRF Attacks in ColdFusion 9"
datePublished: Mon Aug 04 2014 19:23:23 GMT+0000 (Coordinated Universal Time)
cuid: ckrowvc5j0p4ufws14i5e0azt
slug: preventing-csrf-attacks-in-coldfusion-9-ae50cd698279
tags: web-development, security

---


Update April 17, 2012: David Boyer has created a more feature-rich version of the simple scripts I’ve made here. He has actually replicated the CSRF features that come standard in CF10.

Any time your servers are accepting input from the public Internet, you have to be aware of the many types of attacks that can be launched against them. Some examples are [cross-site scripting (XSS)](http://en.wikipedia.org/wiki/Cross-site_scripting) and [SQL injection](http://en.wikipedia.org/wiki/SQL_injection). Today I’m going to talk about [cross-site request forgery (CSRF)](http://en.wikipedia.org/wiki/Cross-site_request_forgery) and, specifically, how to prevent it in ColdFusion 9.

Cross-site request forgery is (as described on [Wikipedia](http://en.wikipedia.org/wiki/Cross-site_request_forgery)):
> *… a type of malicious exploit of a website whereby unauthorized commands are transmitted from a user that the website trusts. Unlike cross-site scripting (XSS), which exploits the trust a user has for a particular site, CSRF exploits the trust that a site has in a user’s browser.*

![Image credit: [Terri Oda](https://cdn.hashnode.com/res/hashnode/image/upload/v1627409822251/sNlr4gzae.html)](https://cdn-images-1.medium.com/max/2000/1*rk_VdqwWqZ331Ms7E3Eziw.jpeg)*Image credit: [Terri Oda](https://www.flickr.com/photos/terrio/)*

What this means is that, unless your server is properly protected, it could accept input from a source other than who you intended. A common way for an attacker to achieve this would be by having a legitimate user, who may already be logged into your site, execute some code unintentionally. This could be as simple as crafting a misleading HTML element such as an image or link. When the code is executed by the user, it would send a request to your site, which would normally not be an issue since you already trust the user. However, the attacker’s code could have the user perform any action he would normally be able to do on your site, such as delete his account, post a comment on a forum, transfer currency, etc.

One way to prevent this is to require the attacker to know something that only your server knows, and sends to your legitimate user when he visits the specific page to perform authenticated actions. This is normally called a CSRF token. [ColdFusion 10 will already have methods for generating and verifying CSRF tokens](http://help.adobe.com/en_US/ColdFusion/10.0/Developing/WSe61e35da8d3185183e145c0d1353e31f559-7ffe.html). But for now, it’s not too hard to do on our own.

Let’s look at a simple self-submitting form:

```
<cfif structKeyExists(form, 'firstName')>
  <cfoutput>
    <p>Thank you for submitting the form #form.firstName# #form.lastName#.</p>

    <p><a href="#CGI.SCRIPT_NAME#">Back to Form</a></p>
  </cfoutput>
<cfelse>
  <cfform>
    <label for="firstName">First name:</label>
    <cfinput name="firstName" type="text"><br>

    <label for="lastName">Last name:</label>
    <cfinput name="lastName" type="text"><br>

    <input type="submit" value="Submit">
  </cfform>
</cfif>
```


This form is vulnerable to a CSRF attack because a malicious user could have a legitimate user unknowingly submit it. To prevent this, we’ll set a CSRF token in the user’s session and then place the token in the form to be submitted. Our token can be any random data. A [universally unique identifier (UUID)](http://en.wikipedia.org/wiki/Universally_unique_identifier) is a good candidate for this because it’s guaranteed to be unique. Within the form:

```
<cfset session.token = createUUID()>

<cfoutput>
  <input type="hidden" name="token" value="#session.token#"></cfoutput>
```


Now, when the form is POSTed, we’ll check to make sure these values match. After checking that the form has been submitted, add:

```
<cfif structKeyExists(session, 'token') and form.token is session.token>
  <p>Thank you for submitting the form #form.firstName# #form.lastName#.</p>
<cfelse>
  <p>There was a problem with your CSRF token!</p>
</cfif>
```


With these modifications in place, the attacker cannot have the user submit the form unintentionally without knowing the randomly-generated value of token. Just to tie any loose ends up, I also like to delete the token session variable when we’re through with it. After verifying the token values:

```
<cfset structDelete(session, 'token')>
```


That’s it. This is how the code should appear altogether:

```
<cfif structKeyExists(form, 'firstName')>
  <cfoutput>
    <cfif structKeyExists(session, 'token') and form.token is session.token>
      <p>Thank you for submitting the form #form.firstName# #form.lastName#.</p>
    <cfelse>
      <p>There was a problem with your CSRF token!</p>
    </cfif>

    <cfset structDelete(session, 'token')>

    <p><a href="#CGI.SCRIPT_NAME#">Back to Form</a></p>
  </cfoutput>
<cfelse>
  <cfform>
    <cfset session.token = createUUID()>

    <cfoutput>
      <input type="hidden" name="token" value="#session.token#">
    </cfoutput>

    <label for="firstName">First name:</label>
    <cfinput name="firstName" type="text"><br>

    <label for="lastName">Last name:</label>
    <cfinput name="lastName" type="text"><br>

    <input type="submit" value="Submit">
  </cfform>
</cfif>
```


In most cases, you won’t have to worry about someone trying to exploit an attack like this, unless you’re developing on a high-profile or currency-driven site. But with only five extra lines of code (and even less on some platforms, including CF10) it doesn’t hurt or complicate things to build in a little extra prevention.

*This post first appeared on my old blog on April 16, 2012.*