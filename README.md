# CSRF Protection in Laravel

## Introduction

Cross-Site Request Forgery (CSRF) is a type of malicious exploit whereby unauthorized commands are performed on behalf of an authenticated user. Laravel makes it easy to protect your application from CSRF attacks.

## Understanding CSRF Vulnerability

Imagine your application has a `/user/email` route that accepts a POST request to change the authenticated user's email address. Without CSRF protection, a malicious website could create a hidden form that submits a POST request to this route on behalf of the user, changing their email address without their consent.

Example of such malicious form:

```html
<form action="https://your-application.com/user/email" method="POST">
    <input type="email" value="malicious-email@example.com" />
</form>

<script>
    document.forms[0].submit();
</script>
