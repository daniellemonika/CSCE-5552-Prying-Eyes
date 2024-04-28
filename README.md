## CSCE 5552: Final Project

**CSCE 5552: Cybersecurity Essentials**

**Group 7**

**Hack the Box:** Prying Eyes (Medium)

https://app.hackthebox.com/challenges/459

**Challenge Description:** Welcome to the Prying Eyes, a "safe space" for those curious about the large organisations that dominate our life. How safe is the site really?

This final project involved capturing the flag for the Hack the Box Prying Eyes web box. Our process of investigation involved reconnaissance, analyzing source files, and researching possible vulnerabilities. Upon initial inspection, the site is identified as using HTTP, having user registration and login, and the ability to upload image files. The user registration and login became a possible point to exploit a SQL injection attack and gain access to database credentials. Subsequent access to the database could lead to system access that allows the flag to be captured. Another possible idea involved using a payload to exploit the file upload feature. All steps were taken within Kali Linux.

## Reconnaissance:
Our group began by scanning for open ports and web application vulnerabilities. We utilized scanning tools, including nmap and Nessus, to further understand the web server. The nmap command only returned port 22 SSH. The Nessus vulnerability scanner only found that the web server was running on HTTP instead of HTTPS.

## Analyzing Source Files:
Our group's next step involved analyzing the source files that are available to download with the box. Within these javascript, dockerfile, and html files, we discovered multiple different tools and systems used by the web server as well as database credentials. This included sqlite and ImageMagick version 7.1.0-33.

## Researching Possible Vulnerabilities:
Keeping the gathered information in mind, our team began by attempting both basic and advanced SQL injection attacks through manual injection and by using the Hydra tool. After no luck with this type of attack, the version of ImageMagick was researched for possible vulnerabilities. The CVE-2022-44268 vulnerability was discovered (https://nvd.nist.gov/vuln/detail/CVE-2022-44268). This vulnerability allows for the disclosure of information to be embedded into a PNG file.

## Process of Capturing the Flag:
The steps taken to capture the flag include creating the malicious PNG file, uploading the malicious file to the website, altering the POST request, and retrieving the flag from the returned image file. In order to generate the malicious PNG file, the ImageMagick Arbitrary File Disclosure (CVE-2022-44268) tool by vulhub (https://github.com/vulhub/vulhub/tree/master/imagemagick/CVE-2022-44268) was used. 

Generate the malicious PNG that points to the flag.txt file within the working directory using the tool by vulhub:
```
sudo python3 ./poc.py generate -o evil.png -r /home/node/app/flag.txt
```

Verify the **tEXt** chunk is set to **profile./home/node/app/flag.txt**:
```
exiv2 -pS evil.png
```
![image](https://github.com/daniellemonika/CSCE-5552-Prying-Eyes/assets/64706142/cde71d41-d5c9-469e-b48a-91820b89f62b)


**After the file was generated, Burp Suite was used to modify the POST request.**

Open Burp Suite in Kali:
```
burpsuite
```

Under 'Proxy', make sure 'Intercept is on' is selected. Then click 'Open browser' to launch the Chromium browser. Navigate to the website _(IP:Port)_ provided through Hack the Box within the Chromium browser, register if an account has not already been created, and login to an account. While intercept is turned on within Burp Suite, the user will need to 'Forward' all requests and responses. Once logged in, create a new post or comment on an existing post.

![image](https://github.com/daniellemonika/CSCE-5552-Prying-Eyes/assets/64706142/c41b3cee-bcc4-469a-b9a6-b873c2abe831)

Modify the POST request to include another parameter, "Background", and alter the value to write to the /uploads directory:
```
Content-Disposition: form-data; name="background"

green -write ./uploads/flag.png
```

![image](https://github.com/daniellemonika/CSCE-5552-Prying-Eyes/assets/64706142/e75bd378-f678-4493-8776-1bcf0754c032)

Navigate to the newly written file by going to file location within a browser:
```
http://IP:Port/uploads/flag.png
```

The image that was uploaded should be shown on the flag.png page. Right-click and download this flie.

Convert the flag.AVIF file from AVIF to PNG:
```
convert flag.AVIF flag.png
```

Extract the metadata from the PNG file:
```
exiftool -b flag.png
```

Copy the long string hex code returned by the extraction into a hex to string calculator _(such as https://cyberchef.org/#recipe=From_Hex('Auto'))_ to obtain the flag.

## Risks of this Vulnerability:
This information disclosure vulnerability can allow for unauthorized access to data and a loss of confidentiality. If a threat actor gains unauthorized access to sensitive data, it may lead to financial and reputational losses.

## Prevention of this Vulnerability:
The CVE-2022-44268 vulnerability can be mitigated against by regular patching and updating of web application tools. Using versions with known vulnerabilities may leave systems open to being exploited.
