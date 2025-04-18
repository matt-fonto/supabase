1. Confirm signup

```html
<!-- Confirmation Signup Email Template -->
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <style>
      body {
        font-family: Arial, sans-serif;
        background: #f4f4f5;
        margin: 0;
        padding: 0;
      }
      .container {
        max-width: 600px;
        margin: 2rem auto;
        background: #ffffff;
        border-radius: 8px;
        overflow: hidden;
        box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
      }
      .header {
        background: #4f46e5;
        color: #ffffff;
        padding: 1rem;
        text-align: center;
      }
      .content {
        padding: 1.5rem;
        color: #333333;
      }
      .button {
        display: inline-block;
        margin: 1rem 0;
        padding: 0.75rem 1.5rem;
        background: #4f46e5;
        color: #ffffff;
        text-decoration: none;
        border-radius: 4px;
      }
      .footer {
        font-size: 0.875rem;
        color: #666666;
        text-align: center;
        padding: 1rem;
      }
    </style>
  </head>
  <body>
    <div class="container">
      <div class="header">
        <h1>Welcome to {{app_name}}!</h1>
      </div>
      <div class="content">
        <p>Hi {{user_name}},</p>
        <p>
          Thanks for signing up. Please confirm your email address by clicking
          the button below:
        </p>
        <a href="{{confirm_url}}" class="button">Confirm Email</a>
        <p>If you didn’t create an account, just ignore this email.</p>
      </div>
      <div class="footer">
        <p>&copy; {{year}} {{app_name}}. All rights reserved.</p>
      </div>
    </div>
  </body>
</html>
```

2. Invite user

```html
<!-- Invite User Email Template -->
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <style>
      body {
        font-family: Arial, sans-serif;
        background: #f4f4f5;
        margin: 0;
        padding: 0;
      }
      .container {
        max-width: 600px;
        margin: 2rem auto;
        background: #ffffff;
        border-radius: 8px;
        overflow: hidden;
        box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
      }
      .header {
        background: #10b981;
        color: #ffffff;
        padding: 1rem;
        text-align: center;
      }
      .content {
        padding: 1.5rem;
        color: #333;
      }
      .button {
        display: inline-block;
        margin: 1rem 0;
        padding: 0.75rem 1.5rem;
        background: #10b981;
        color: #fff;
        text-decoration: none;
        border-radius: 4px;
      }
      .footer {
        font-size: 0.875rem;
        color: #666;
        text-align: center;
        padding: 1rem;
      }
    </style>
  </head>
  <body>
    <div class="container">
      <div class="header"><h1>You’re Invited!</h1></div>
      <div class="content">
        <p>Hello,</p>
        <p>
          {{inviter_name}} has invited you to join
          <strong>{{app_name}}</strong>. Click below to create your account:
        </p>
        <a href="{{invite_url}}" class="button">Accept Invitation</a>
        <p>If you weren’t expecting this, you can safely ignore this email.</p>
      </div>
      <div class="footer"><p>&copy; {{year}} {{app_name}}</p></div>
    </div>
  </body>
</html>
```

3. Magic link

```html
<!-- Magic Link Email Template -->
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <style>
      body {
        font-family: Arial, sans-serif;
        background: #f4f4f5;
        margin: 0;
        padding: 0;
      }
      .container {
        max-width: 600px;
        margin: 2rem auto;
        background: #ffffff;
        border-radius: 8px;
        box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
      }
      .header {
        background: #f59e0b;
        color: #fff;
        padding: 1rem;
        text-align: center;
      }
      .content {
        padding: 1.5rem;
        color: #333;
      }
      .button {
        display: inline-block;
        margin: 1rem 0;
        padding: 0.75rem 1.5rem;
        background: #f59e0b;
        color: #fff;
        text-decoration: none;
        border-radius: 4px;
      }
      .footer {
        font-size: 0.875rem;
        color: #666;
        text-align: center;
        padding: 1rem;
      }
    </style>
  </head>
  <body>
    <div class="container">
      <div class="header"><h1>Your Magic Link</h1></div>
      <div class="content">
        <p>Hi {{user_name}},</p>
        <p>
          Use the link below to securely sign in. It expires in
          {{expiration_minutes}} minutes.
        </p>
        <a href="{{magic_link_url}}" class="button">Sign In Now</a>
        <p>If you didn’t request this, please ignore.</p>
      </div>
      <div class="footer"><p>&copy; {{year}} {{app_name}}</p></div>
    </div>
  </body>
</html>
```

4. Change email address

```html
<!-- Change Email Address Email Template -->
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <style>
      body {
        font-family: Arial, sans-serif;
        background: #f4f4f5;
        margin: 0;
        padding: 0;
      }
      .container {
        max-width: 600px;
        margin: 2rem auto;
        background: #fff;
        border-radius: 8px;
        box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
      }
      .header {
        background: #3b82f6;
        color: #fff;
        padding: 1rem;
        text-align: center;
      }
      .content {
        padding: 1.5rem;
        color: #333;
      }
      .button {
        display: inline-block;
        margin: 1rem 0;
        padding: 0.75rem 1.5rem;
        background: #3b82f6;
        color: #fff;
        text-decoration: none;
        border-radius: 4px;
      }
      .footer {
        font-size: 0.875rem;
        color: #666;
        text-align: center;
        padding: 1rem;
      }
    </style>
  </head>
  <body>
    <div class="container">
      <div class="header"><h1>Confirm Email Change</h1></div>
      <div class="content">
        <p>Hello {{user_name}},</p>
        <p>
          We received a request to change your email to
          <strong>{{new_email}}</strong>. Click below to confirm:
        </p>
        <a href="{{change_email_url}}" class="button">Confirm Change</a>
        <p>If this wasn’t you, contact support immediately.</p>
      </div>
      <div class="footer"><p>&copy; {{year}} {{app_name}}</p></div>
    </div>
  </body>
</html>
```

5. Reset password

```html
<!-- Reset Password Email Template -->
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <style>
      body {
        font-family: Arial, sans-serif;
        background: #f4f4f5;
        margin: 0;
        padding: 0;
      }
      .container {
        max-width: 600px;
        margin: 2rem auto;
        background: #fff;
        border-radius: 8px;
        box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
      }
      .header {
        background: #ef4444;
        color: #fff;
        padding: 1rem;
        text-align: center;
      }
      .content {
        padding: 1.5rem;
        color: #333;
      }
      .button {
        display: inline-block;
        margin: 1rem 0;
        padding: 0.75rem 1.5rem;
        background: #ef4444;
        color: #fff;
        text-decoration: none;
        border-radius: 4px;
      }
      .footer {
        font-size: 0.875rem;
        color: #666;
        text-align: center;
        padding: 1rem;
      }
    </style>
  </head>
  <body>
    <div class="container">
      <div class="header"><h1>Reset Your Password</h1></div>
      <div class="content">
        <p>Hey {{user_name}},</p>
        <p>
          Click the button below to reset your password. This link expires in
          {{expiration_minutes}} minutes.
        </p>
        <a href="{{reset_password_url}}" class="button">Reset Password</a>
        <p>If you didn’t request this, please ignore.</p>
      </div>
      <div class="footer"><p>&copy; {{year}} {{app_name}}</p></div>
    </div>
  </body>
</html>
```
