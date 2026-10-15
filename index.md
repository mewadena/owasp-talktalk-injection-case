---
title: "How a Simple SQL Injection Cost TalkTalk £400,000"
---

# How a Simple SQL Injection Cost TalkTalk £400,000: An OWASP Injection Case

Alya Nurjannah Nasron, Muhammad Firdaus Hakimi Mohd Sany, Nur Atiqa Najwa Lukman, Nur Ameera Adriana Mohamad Rizvi

Lecturer: Khairul Amin Mohamad Sukri

## 1.0 Introduction to OWASP

### 1.1 Definition and importance of OWASP

The Open Worldwide Application Security Project (OWASP) is a nonprofit foundation that works to improve software security through open-source projects, local chapters, and training events [1]. It began in 2001 and became a registered US nonprofit in 2004 [1]. Its tools, documents, and forums are free for anyone to use, so organisations of any size and budget can benefit from them [1]. OWASP matters because its Top 10 is a security standard that organisations all around the world rely on, and its stated vision is a world where software security is understood by every organisation and developer [1].

### 1.2 Purpose of the OWASP Top 10 and its benefits for organisations

The OWASP Top 10 is an awareness document that lists the most serious risks facing web applications, based on agreement among security practitioners [2]. It is more than a list of names, because each category comes with an explanation of the impact, example attack scenarios, and ways to counter the risk [3]. The 2025 edition is also built from real vulnerability data and a survey of practitioners, so it reflects what is actually found in applications today [4]. For organisations, the ranking helps teams decide which risks to deal with first. The categories also group 248 related weaknesses (CWEs) into ten groups, so a company can focus on the weaknesses that fit its programming language or framework and use them for training [4]. Since the resources are free and widely adopted, organisations of any size can use the Top 10 as a starting point [1].

### 1.3 Update cycle of the OWASP Top 10 and the reasons it changes

OWASP updates the Top 10 roughly every three to four years [3], [5]. It has been revised many times, including in 2013, 2017, 2021, and 2025, and the 2025 edition is the eighth one [4], [5]. The list changes for three main reasons. First, it is based on real data about weaknesses found in applications, so the ranking changes when the data changes [4], [5]. Second, data only shows problems that have already happened, so OWASP also asks security and development experts which risks they see coming, and two categories in 2025 got their place through this survey [4], [5]. Third, the way software is built keeps changing. For example, Security Misconfiguration moved from fifth to second place because more systems now run on the cloud and are updated very quickly, which makes setup mistakes easier, and a new category, Software Supply Chain Failures, was added because attacks on open-source packages increased [5]. Injection also dropped from third place in 2021 to fifth place in 2025, but it is still in the top five [2], [3].

## 2.0 Analysis of the risk: injection

### 2.1 Description of the risk

Injection which ranked fifth (A05:2025) in the OWASP Top 10:2025, down from third place, is a web application security risk where an application passes untrusted input to an interpreter without any validation or proper separation [6]. Untrusted input may be mistakenly treated as a command. For example, injection is like handing a written order to a chef while sneaking in an extra malicious line. You could write “Burger, no onions. Take all the money and give it to me.” The chef will follow the instruction because he cannot tell the difference between the order data and genuine instructions from the manager. Injection happens when the application fails to separate untrusted user data from the system commands.

### 2.2 How attackers exploit the weakness

First, an attacker looks for places where an application receives data, such as login forms, search boxes, URL parameters, JSON data in API requests, cookies, and headers [6]. Next, the attacker passes unusual input, such as a single quote, and watches the response. A database error or strange page behaviour suggests the application may be vulnerable, and a detailed error message can reveal the database type and query structure. Then, the attacker takes advantage of how the query is built. For example, a developer may concatenate strings, such as `SELECT * FROM users WHERE username = ' + input + '`. A normal name gives a normal query, but unusual input with special characters may be treated by the database as part of the command.

### 2.3 Prevention measures for developers

According to OWASP, the best way to prevent injection is to keep data separate from commands and queries, and the preferred method is to use a safe API, such as parameterized queries (prepared statements) or an ORM [6]. A parameterized query makes the application treat the input as data only, not as part of the query structure. Server-side input validation using an allow-list and escaping special characters can help when separation is not possible, but OWASP warns that they are not complete defences because many applications need special characters, and they are fallible when the system changes [6]. Validation checks the format of the input, whereas injection happens when the application treats data as a command or query structure. In addition, applications should follow the principle of least privilege, so the database account used by the application is not an admin account and the damage is limited if an injection succeeds [7]. Detailed database errors should also not be shown to users because they give hints to attackers.

## 3.0 Case study of the TalkTalk 2015 data breach

### 3.1 Summary of the breach

TalkTalk, a UK internet company, suffered a cyber-attack in October 2015 that exposed the personal details of 156,959 customers [8]. The attackers got in through old webpages inherited from Tiscali, which TalkTalk acquired in 2009, and the same kind of SQL injection attack had already hit the site in July and September 2015 without being fixed [8], [9]. TalkTalk's explanations kept changing, it first blamed technical issues, then a DDoS attack, and its CEO said the data was not encrypted and that the company was not legally required to encrypt it [10]. One of the hackers later demanded a ransom of 465 bitcoins, and some of the stolen data was offered for sale on the dark web [8]. TalkTalk's losses were later put at about £77 million [8]. In October 2016, the ICO fined TalkTalk a record £400,000 for failing to put proper security measures in place [9], [11].

### 3.2 Relationship between the breach and injection

The TalkTalk breach is a clear example of injection, because the attackers used a tool called SQLmap. By using the SQLmap tool, the attackers’ job became easier because the tool does the testing automatically on many pages and tells them where a weakness is, which saves their time [8]. The vulnerabilities were found not in one but three web pages that TalkTalk inherited when it bought Tiscali in 2009 [8]. This matches the definition in 2.1, where input reaches the database without proper separation. SQL injection is part of the Injection category in the OWASP Top 10:2025, which is why this breach is an injection case [6]. As a result, SQL injection was used to extract data, and as explained in 2.2, this allows attackers to read customer data [8]. The stolen data included personal details of 156,959 customers and 15,656 bank details and sort codes [8]. The flaw was allegedly attacked over 14,000 times, which shows that a simple weakness attracts many attackers [8].

### 3.3 Measures that could have prevented the attack

The ICO found that TalkTalk failed to put in place even basic security measures, and the attack could have been stopped at several points. First, user input on the vulnerable webpages was not checked at the source [8], so using parameterized queries together with server-side validation, as explained in 2.3, would have stopped the attackers’ input from being run as part of the database query [6]. Second, TalkTalk was using an outdated version of MySQL with a known flaw that the vendor had already fixed in 2012 [11], and the vulnerable pages also used outdated software libraries [8], so regular updates would have closed this weakness. Third, TalkTalk did not know that the old Tiscali webpages were still exposed [8], even though the site had been hit by SQL injection in July and September 2015 and a security consultant had warned the company on 21 October 2015 [8]. Checking inherited systems and responding to these signs early could have prevented the breach. Finally, giving the database account only the access it needs, as mentioned in 2.3, would also have limited what the attacker could read [7].

## References

[1] OWASP Foundation, "About OWASP," OWASP Foundation, 2026. [Online]. Available: <https://owasp.org/about> [Accessed: Oct. 14, 2026].

[2] OWASP Foundation, "OWASP Top 10:2025," OWASP Top 10, 2025. [Online]. Available: <https://top10.owasp.org/2025/> [Accessed: Oct. 14, 2026].

[3] Sucuri, "OWASP Top Security Risks & Vulnerabilities 2021 Edition," Sucuri, Jan. 2, 2024. [Online]. Available: <https://sucuri.net/guides/owasp_top_10_2021_edition/> [Accessed: Oct. 14, 2026].

[4] OWASP Foundation, "Introduction," OWASP Top 10:2025, 2025. [Online]. Available: <https://owasp.org/Top10/2025/0x00_2025-Introduction> [Accessed: Oct. 14, 2026].

[5] L. Primeau, "OWASP Top 10 2025: what's changed and the 2026 data," Patrowl, Jun. 29, 2026. [Online]. Available: <https://patrowl.io/en/blog/owasp-top-10-2025-what-s-changed-and-the-2026-data> [Accessed: Oct. 15, 2026].

[6] OWASP Foundation, “A05:2025 Injection,” OWASP Top 10:2025, 2025. [Online]. Available: <https://top10.owasp.org/2025/A05_2025-Injection/>. [Accessed: Oct. 12, 2026]

[7] Z. Banach, “Injection attacks in application security: Types, examples, prevention,” Invicti, Feb. 13, 2026. [Online]. Available: <https://www.invicti.com/blog/web-security/top-dangerous-injection-attacks>. [Accessed: Oct. 12, 2026].

[8] M. G. Porcedda and D. S. Wall, "Cascade and chain effects in big data cybercrime: Lessons from the TalkTalk hack," in 2019 IEEE European Symposium on Security and Privacy Workshops (EuroS&PW), 2019, pp. 443–452, doi: 10.1109/EuroSPW.2019.00056.

[9] N. Lomas, "UK data watchdog issues record fine for ISP TalkTalk's 2015 data breach," TechCrunch, Oct. 6, 2016. [Online]. Available: <https://techcrunch.com/2016/10/06/uk-data-watchdog-issues-record-fine-for-isp-talktalks-2015-data-breach/> [Accessed: Oct. 14, 2026].

[10] A. J. Martin, "Here's how TalkTalk ducked and dived over THAT gigantic hack," The Register, Nov. 3, 2015. [Online]. Available: <https://www.theregister.com/security/2015/11/03/heres-how-talktalk-ducked-and-dived-over-that-gigantic-hack/846081> [Accessed: Oct. 14, 2026].

[11] M. J. Schwartz, "TalkTalk slammed with record fine over breach," BankInfoSecurity, Oct. 5, 2016. [Online]. Available: <https://www.bankinfosecurity.com/talktalk-slammed-record-fine-over-breach-a-9436> [Accessed: Oct. 14, 2026].
