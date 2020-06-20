# Information Gathering

So what are the differences between Active recon and Passive recon? Well, as explained in the other note of the five stages of ethical hacking, Active recon is any kind of information gathering that is being directed towards the organization. Putting it in abstract, basically every kind of info gathering that talks to the organization's server's. For example, nmap is considered to be an Active approach since it scans the ports of a machine/network of the organization.

Passive recon is when an attacker uses all kinds of search engines, social networks and more in order to **find information that is already out there**. No tampering with existing servers, services, machines, people, etc.

# **Information Gathering**

Identifying our target - Say we were given an IP address. This IP address is to be assessed upon and to find all its vulnerabilities and loose ends that lead to vulnerable servers/machines/services/people.

Before actually executing any kind of recon, we need to validate that the IP address is in fact the IP address of the organization we were requested to vuln assess.

For this kind of method, we would use [shodan.io](http://shodan.io), nslookup, whois, etc. Anything that would give us the stamp on the organization authenticity. 

There were few incidents in the past where professional Penetration Testers were given a malformed IP address. In those incidents, the attackers did not bother to really validate their targets, resulting in attacking out of scope machines and services, resulting in utter chaos and confusion with possible legal prosecution.

**This is to be avoided at all costs.** 

### Finding a client - Bug Bounties

[#1 Crowdsourced Cybersecurity Platform | Bugcrowd](https://www.bugcrowd.com/)

Make sure that whenever you start some kind of a program, you check all the details about which domains are OK to attack and which aren't. Refer to the "In-Scope" list.

### E-Mail Address Gathering with [Hunter.io](http://hunter.io)

[Hunter.io](http://hunter.io/), A.K.A Email Hunter, is an email hunter tool that helps marketers find the contact information associated with any domain. This is ideal for companies that use cold emailing as a way to fill their pipeline. Furthermore, Email Hunter can be used to verify emails and do bulk tasks.

(Taken from: [https://carminemastropierro.com/hunter-io-2/#:~:text=Hunter.io%2C A.K.A Email Hunter,emails and do bulk tasks](https://carminemastropierro.com/hunter-io-2/#:~:text=Hunter.io%2C%20A.K.A%20Email%20Hunter,emails%20and%20do%20bulk%20tasks).)

With this wonderful tool, it is possible to execute Passive recon on some organization. In the examples of the [udemy.com](http://udemy.com) course, taught by Heath Adams, he uses tesla.com.

For the sake of not wasting my free searches, I took a screenshot:

![Information%20Gathering%2030dc013403604ebca3df4b78680bdac1/Untitled.png](Information%20Gathering%2030dc013403604ebca3df4b78680bdac1/Untitled.png)

In this screenshot, we are able to see all the E-Mail results that were fetched from [hunter.io](http://hunter.io) search mechanism, after inputting tesla.com.

In the screenshot, it is seen that [hunter.io](http://hunter.io) is presenting us with common email patterns that the organization (in our case - Tesla) uses.

You can choose whether to target the Support, Sales or Communication teams. Whichever suits your needs.

Personally, I would target the support/sales since most of the time people who occupy these positions are people in the start of their career, trying to make some money or starting their career in IT, which results in probable lack of "cyber awareness".

**This is the foundation for a social engineering attack.** 

[Hunter.io](http://hunter.io) is useful for searching for an email pattern and brute-forcing that email pattern with lists of names, common email addresses and common passwords. This would be possible only if the login form would be abusable in this kind of way. Most of login forms today are pretty sophisticated and stand behind a powerful firewall (cloudflare for example) that knows how to detect brute-forcing by blocking the requesting host, if the requesting host is flooding the server with login attempt, but with different users.

### Gathering Breached Credentials with Breach-Parse

Breach-Parse is a tool to brute-force logins with emails. 

The tool takes 2 arguments - @domainame.com email-list-of-domain - ./Breach-Parse @tesla.com tesla.txt

The tool returns 3 lists. Master - Username:Password, Usernames.txt and Passwords.txt

### Utilizing "theharvester"

theharvester is a passive recon tool that goes to popular data sources (google, yahoo, bing and more of the sort...) and harvests the data relevant to your input. 

Example: syntax - theharvester -d [tesla.com](http://tesla.com) -l 500 -b google -f /root/home/output.txt

-d [domain]

-l [how many searches to perform, how many results to output. In our case here it's 500]

-b [search engine to look in, 'Google' for example, has a dorking option, worth checking it out]

-f [output result source path]

![Information%20Gathering%2030dc013403604ebca3df4b78680bdac1/Untitled%201.png](Information%20Gathering%2030dc013403604ebca3df4b78680bdac1/Untitled%201.png)

These are the results for the command above.

## Hunting Subdomains
