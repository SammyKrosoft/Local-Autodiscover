# Local-Autodiscover
Using Local Autodiscover file with Outlook

Outlook 2016 and later will use the below framework to get to the Autodiscover information - we'll focus on Step 2 here, which is Outlook checking for a PreferLocalXML registry key to try getting to a local Autodiscover.xml file for a user with a specific SMTP domain:

![image](https://user-images.githubusercontent.com/33433229/168157464-ff037022-c85e-4341-b089-3effe8ab605a.png)


--------------------------------------------------------------------------------------------------------------------------------------------

For a user with an SMTP e-mail address with a specific @contoso.ca SMTP domain, we can tell Outlook to use a local autodiscover.xml file to either direct it to another Autodiscover URL (redirect) or to simply give Outlook specific URL to go to for the MAPI HTTP, RPC HTTP, IMAP and/or POP settings.

- First we must tell Outlook to prefer a local XML file before attempting SCP lookup or HTTPS root domain or even a DNS lookup for an autodiscover.contoso.ca URL. To do that we just have to add the ```PreferLocalXML``` 

- Second, you will also need to specify the domain that applies (aka for which SMTP domain would you like to provide a local XML file configuration) and the path to the local Autodiscover.xml file => that way, for the other Outlook profiles for users with other SMTP addresses, Outlook will not try to get a local Autodiscover.XML file. That will be used only for SMTP domains you specify on the below registry key sample - actually the registry key's name is the SMTP domain you want the local Autodiscover.xml instructions to be applied to.

For example, to provide contoso.ca e-mail address settings from a local XML file, you could configure the following registry value. Replace xx.x with the value corresponding to your Outlook version:

```
Key: HKEY_CURRENT_USER\SOFTWARE\Microsoft\Office\xx.x\Outlook\AutoDiscover
Key name: contoso.ca
Key type: REG_SZ
Value:    C:\Autodiscover\autodiscover.xml
```

In this example, the XML settings file is located here: C:\Autodiscover\autodiscover.xml, and it is for contoso.ca as it's the registry key name.

--------------------------------------------------------------------------------------------------------------------------------------------
Content of Autodiscover.xml for autodiscover URL redirection:

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
