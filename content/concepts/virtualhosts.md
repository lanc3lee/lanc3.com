how are virtualhosts different from subdomains?

subdomains usually have a different IP address
virtualhosts don't

example: Flight box on hackthebox

/etc/hosts
10.129.228.120 flight.htb school.flight.htb

`school.flight.htb` is a **subdomain** in terms of naming structure, but the mechanism that makes it work on a single IP address is called **Virtual Hosting**

## What is a Virtual Host (vHost)?

A **Virtual Host** is a configuration on a web server (like Apache or Nginx) that allows multiple websites to run on a single IP address.

Imagine an apartment building at **10.129.228.120**.

- If you just send a letter to the building address (**flight.htb**), the lobby (default site) receives it.
    
- If you want to reach the "School" office (**school.flight.htb**), you have to specify that name on the envelope so the concierge knows which room to send you to.
    

### The "Why" behind `/etc/hosts`

When you type `school.flight.htb` into your browser:

1. **DNS Lookup:** Your computer checks `/etc/hosts`. If it's not there, it doesn't know which IP to talk to.
    
2. **The Request:** Once it finds the IP, your browser sends an HTTP request to `10.129.228.120`.
    
3. **The Host Header:** Inside that request, your browser includes a "Host Header": `Host: school.flight.htb`.
    

---

## Why can't you see it with only `flight.htb`?

If you only have `10.129.228.120 flight.htb` in your hosts file:

1. **Navigation:** You type `http://school.flight.htb`. Your computer says, "I don't know where that is," and fails immediately.
    
2. **IP Browsing:** If you type `http://10.129.228.120` directly, the server receives the request but doesn't see a "Host" name it recognizes. It will simply serve you the **Default Page** (which is usually the `flight.htb` main site).
    

The server on the "Flight" box is specifically configured to look for the name `school.flight.htb`. If that exact string isn't sent in the HTTP header, the server won't "route" you to the school-specific files and code.

---

## The Difference: Subdomain vs. VHost

| **Feature**                                                                                                                                                                                                                                                                                                                                                                                                                                                                        | **Subdomain**                              | **Virtual Host**                              |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------ | --------------------------------------------- |
| **What it is**                                                                                                                                                                                                                                                                                                                                                                                                                                                                     | A DNS/Naming concept (the `school.` part). | A Web Server configuration concept.           |
| **Purpose**                                                                                                                                                                                                                                                                                                                                                                                                                                                                        | To organize the hierarchy of a domain.     | To host multiple domains on 1 server/IP.      |
| Pro-Tip for HTB<br><br>Often, HTB boxes hide "Virtual Hosts" that aren't subdomains (e.g., `dev.flight.htb` or even a totally different name). Since these aren't in a real DNS, tools like `ffuf` or `gobuster` are used to "fuzz" the Host Header to see if the server responds with a different page for a different name.<br><br>**Your `/etc/hosts` should look like this:**<br><br>Plaintext<br><br>```<br>10.129.228.120  flight.htb school.flight.htb<br>```**Visibility** | Publicly visible in the URL.               | Invisible to the user; handled by the server. |
