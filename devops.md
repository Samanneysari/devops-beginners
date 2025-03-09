# Devops

Topic of this Course:

* 1-http/https,Nginx

* 2-Docker

* 3-Git/Gitlab CI

* 4-Bash scripting

* 5-Ansible

* 5-Prometheus/Grafana

## http, https, Nginx

### What is HTTP?
HTTP, or Hypertext Transfer Protocol, is the foundation of data communication on the World Wide Web. It’s an application-layer protocol used for transmitting hypermedia documents, like HTML, between clients (like your browser) and servers. Basically, it’s how your computer asks for and gets web pages.
* Downside: It’s not secure—data can be intercepted and read by anyone.

### What is HTTPS?
HTTPS, or Hypertext Transfer Protocol Secure, is HTTP with a security upgrade. It uses SSL/TLS encryption to protect the data being sent. This means your info (like passwords or credit card numbers) stays private and safe.

* Upside: It’s secure, trusted, and shows a padlock in your browser.

### HTTP vs. HTTPS: Key Differences

| Feature         | HTTP (Hypertext Transfer Protocol) | HTTPS (Hypertext Transfer Protocol Secure) |
|---------------|--------------------------------|----------------------------------|
| **Security**  | Not secure                   | Secure                           |
| **Encryption** | No (data is plain)           | Yes (data is locked)             |
| **Padlock Icon** | No (browser warning possible) | Yes (shows trust)               |
| **Best For**  | Basic, non-sensitive pages    | Logins, payments, personal info |