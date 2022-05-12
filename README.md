# Using Local Autodiscover file with Outlook

Outlook 2016 and later will use the below framework to get to the Autodiscover information - we'll focus on Step 2 here, which is Outlook checking for a ```PreferLocalXML``` registry key to try getting to a local Autodiscover.xml file for a user with a specific SMTP domain:

![image](https://user-images.githubusercontent.com/33433229/168157464-ff037022-c85e-4341-b089-3effe8ab605a.png)

You can find the above procedure on the below link:
https://support.microsoft.com/en-us/topic/outlook-2016-implementation-of-autodiscover-0d7b2709-958a-7249-1c87-434d257b9087

--------------------------------------------------------------------------------------------------------------------------------------------

For a user with an SMTP e-mail address with a specific @contoso.ca SMTP domain, we can tell Outlook to use a local autodiscover.xml file to :
- either direct it to another Autodiscover URL (put "redirectURL" information inside the file) 
- or to simply give Outlook specific URL to go to for the MAPI HTTP, RPC HTTP, IMAP and/or POP settings (put MAPIHTTP, RPCHTTP, IMAP and/or POP information inside the file)

## Tell Outlook to look for a Local XML file first

First, we must tell Outlook to prefer a local XML file before attempting SCP lookup or HTTPS root domain or even a DNS lookup for an autodiscover.contoso.ca URL. To do that we just have to add the ```PreferLocalXML``` 

## Specify which domain will use which autodiscover.xml file path

Second, you will need to specify to which SMTP domain you want to configure a local Autodiscover.xml file, and also specify the path to the local Autodiscover.xml file you want to use for users with that SMTP domain => that way, for the other Outlook profiles for users with other SMTP addresses, Outlook will not use that local Autodiscover.XML file but it will either look for another registry key with the domain name, or continue its process to locate autodiscover via SCP, or DNS or Office 365. To be able to achieve that, the new registry key's name is the SMTP domain you want the local Autodiscover.xml instructions to be applied to.

For example, to provide a user with a *contoso.ca* SMTP domain (aka an e-mail address of type UserName@contoso.ca) settings from a specific local autodiscover.xml file located inside the ```c:\autodiscover\``` folder, you could configure the following registry value. Replace xx.x with the value corresponding to your Outlook version:

```
Key: HKEY_CURRENT_USER\SOFTWARE\Microsoft\Office\xx.x\Outlook\AutoDiscover
Key name: contoso.ca
Key type: REG_SZ
Value:    C:\Autodiscover\autodiscover.xml
```

In the above example, the XML settings file is located here: *C:\Autodiscover\autodiscover.xml*, and it is for *contoso.ca* as it's the registry key name.

## Create the autodiscover.xml file

And third, you will need to create an Autodiscover.xml file on the path specified on the above registry key, for example C:\Autodiscover\autodiscover.xml

Content of Autodiscover.xml for autodiscover URL redirection: on the below example, we specify the action to be of type "redirectUrl", so that the value will be a https: URL that the client should use to obtain the Autodiscover settings or a http: URL that the client should use for further redirection.

```
<?xml version="1.0" encoding="utf-8" ?>
<Autodiscover xmlns="https://schemas.microsoft.com/exchange/autodiscover/responseschema/2006">
<Response xmlns="https://schemas.microsoft.com/exchange/autodiscover/outlook/responseschema/2006a">
<Account>
<AccountType>email</AccountType>
<Action>redirectUrl</Action>
<RedirectUrl>https://autodiscover.hoster.com/autodiscover/autodiscover.xml</RedirectUrl>
</Account>
</Response>
</Autodiscover>
```

Source: [Plan to automatically configure user accounts in Outlook 2010](https://docs.microsoft.com/en-us/previous-versions/office/office-2010/cc511507(v=office.14)?redirectedfrom=MSDN)
