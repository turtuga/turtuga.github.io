---
layout: post
title: "How I found vulnerabilities in SMIG from some Portuguese Municipalities"
date: 2018-11-01 01:00:00 +0000
author: Ricardo Soares
---
In a visit at Municipality of Oliveira de Frades website ([http://cm-ofrades.com/]) I see a link to other website called "Sistema Municipal de Informação Geográfica" ([http://sig.cm-ofrades.com]). This website is protected by username and password but...

![SIG]({{ site.baseurl }}/images/2018-11-01/sig.png "SIG"){: .center-image .max-400 }

### The vulnerability
One of the major shortcomings that immediately hit me was the lack of secure communication **HTTPS** ([https://www.owasp.org/index.php/SSL_Best_Practices](https://www.owasp.org/index.php/SSL_Best_Practices)) which today becomes indispensable.

![HTTPS]({{ site.baseurl }}/images/2018-11-01/no_ssl.png "HTTPS"){: .center-image }

Eventually, I tested the system for attacks known as **SQL Injection** ([https://www.owasp.org/index.php/SQL_Injection](https://www.owasp.org/index.php/SQL_Injection)) in the username and password.

![LOGIN]({{ site.baseurl }}/images/2018-11-01/query_login.png "LOGIN"){: .center-image }


Well, the system was vulnerable to **SQL Injection**. After the vulnerability was exploited, I was able to login to the platform as Administrator.
With an administrator login I was able to query users as well their email addresses, edit or remove them.

![USERS]({{ site.baseurl }}/images/2018-11-01/gestao_users.png "USERS"){: .center-image }


When browsing the platform there are several licensing requests that can be consulted and/or removed and personal data such as _names, addresses, telephone numbers and NIF's_.

![PLANTAS]({{ site.baseurl }}/images/2018-11-01/plantas.png "PLANTAS"){: .center-image }


After a brief survey I noticed that several Municipalities use the same platform, such as **Oliveira de Frades, Vouzela, São Pedro do Sul, Nelas, Tondela, Carregal do Sal, Mangualde, Santa Comba Dão, Penalva do Castelo, Sátão, Aguiar da Beira, Castro Daire, Penalva do Castelo** and others.

![DADOS PESSOAIS]({{ site.baseurl }}/images/2018-11-01/dados_pessoais.png "DADOS PESSOAIS"){: .center-image }


### In conclusion
Following a responsible disclosure model, I reported the vulnerability to the developers of the application. Timeline of events:
* 28 October 2018 - Discovery of the vulnerability and alerted the Municipality of Oliveira de Frades
* 29 October 2018 - Alerted the Municipalities of Vouzela, Nelas and Carregal do Sal (last one alerted by [André Almeida](https://andre-almeida.pt))
* 30 October 2018 - Alerted the company responsible for the platform primelayer
* 31 October 2018 - Vulnerability has been fixed

[http://cm-ofrades.com/]: http://cm-ofrades.com
[http://sig.cm-ofrades.com]: http://sig.cm-ofrades.com

