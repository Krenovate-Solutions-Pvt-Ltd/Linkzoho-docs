1. The checks and prerequisites screens looks like below
![Checks](image-8.png)
2. This screen helps you determine if you have everything in place to connect LinkZoho to your Zoho Books account
3. You need 3 things
    - An active ZohoBooks or ZohoOne subscription 
    - A Zoho Client having ZohoBooks API credentials (read Zoho API Client Credential point)
    - And your ZohoBooks Organization ID

## **Zoho API Client Credential Setup**

1. Visit the Zoho developer console platform- 
https://accounts.zoho.com/developerconsole
2. Login using your Zoho credentials
3. Once logged in Press the button ‘+Add Client’ on the top right corner,
as shown in the image below <br/>
![Add Client](image-4.png)
4. Choose client type as ‘Server based applications’, as shown in the image below
![Add Client](image-3.png)
5. Enter New client details as follows, refer the image below
    - Client name of your choice
    - Client type- server based
    - Your store homepage url in Homepage URL
    - In authorized redirect uris, enter the homepage url followed by ‘/wp-admin’ refer the image below
    - Press create to create a Zoho API client
    ![Add Client](image-5.png)
6. Access client Id and client secret , refer the image below
![Add Client](image-6.png)
7. You have successfully created a new Zoho API client, don't change anything under settings


