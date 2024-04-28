# CSCE 5552: Final Project

**CSCE 5552: Cybersecurity Essentials**

**Group 7**

**Hack the Box:** Prying Eyes (Medium)

https://app.hackthebox.com/challenges/459

**Challenge Description:** Welcome to the Prying Eyes, a "safe space" for those curious about the large organisations that dominate our life. How safe is the site really?

This final project involved capturing the flag for the Hack the Box Prying Eyes web box. Our process of investigation involved reconnaissance, analyzing source files, and researching possible vulnerabilities. Upon initial inspection, the site is identified as using HTTP, having user registration and login, and the ability to upload image files. The user registration and login became a possible point to exploit a SQL injection attack and gain access to database credentials. Subsequent access to the database could lead to system access that allows the flag to be captured. Another possible idea involved using a payload to exploit the file upload feature. 

**Reconnaissance:**
Our group began by scanning for open ports and web application vulnerabilities. We utilized scanning tools, including nmap and Nessus, to further understand the web server. The nmap command only returned port 22 SSH. The Nessus vulnerability scanner only found that the web server was running on HTTP instead of HTTPS.

**Analyzing Source Files:**
Our group's next step involved analyzing the source files that are available to download with the box. Within these javascript, dockerfile, and html files, we discovered multiple different tools and systems used by the web server as well as database credentials. This included sqlite and ImageMagick version 7.1.0-33.

**Researching Possible Vulnerabilities:**
