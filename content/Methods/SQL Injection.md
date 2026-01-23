

For login pages, steps to try

1) use different usernames, with random passwords to see if they would return a 
   "wrong username" error message. 
   If yes, we can spam to see which usernames / login names exist
   If none (all showing "invalid password"), goto (2)
   
2) **Error Test** 
   input`'` in login name field
   Look for "SQL Syntax" or 500 Errors.
   if such errors exist, shows SQL injection could work
   
3) **Bypass Test** ( aka **SQL Injection Authentication Bypass**.)
   input `admin' #`
   See if it ignores the password check.
   
4) **Logic Test**
   input `' OR 1=1--`
   Log in without a valid password.
   
5) **Time Test**`' AND SLEEP(5)--`Force a delay in the server response.