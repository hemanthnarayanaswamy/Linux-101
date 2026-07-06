# The `dig` Linux Command

`dig` command stands for Domain Information Groper. It retrieves information about DNS name servers. Network administrators use it to verify and troubleshoot DNS problems and perform DNS lookups.

```bash
dig [@server] [name] [type] [options]

# @server (optional): Specifies a DNS server to query (e.g., @8.8.8.8 for Google Public DNS).
# name: The domain name or IP address to query.
# type (optional): Specifies the type of DNS record to retrieve (e.g., A, MX, NS).
# options (optional): Additional flags to modify the output.
```

<table><thead><tr><th>Option</th><th>Description</th></tr></thead><tbody><tr><td><code class="language-plaintext highlighter-rouge">-f &lt;filename&gt;</code></td><td>Reads queries from a file.</td></tr><tr><td><code class="language-plaintext highlighter-rouge">-t &lt;type&gt;</code></td><td>Specifies the query type (e.g., <code class="language-plaintext highlighter-rouge">A</code>, <code class="language-plaintext highlighter-rouge">MX</code>, <code class="language-plaintext highlighter-rouge">CNAME</code>, <code class="language-plaintext highlighter-rouge">NS</code>, <code class="language-plaintext highlighter-rouge">TXT</code>, <code class="language-plaintext highlighter-rouge">SOA</code>, etc.).</td></tr><tr><td><code class="language-plaintext highlighter-rouge">-x &lt;IP&gt;</code></td><td>Performs a reverse DNS lookup for the specified IP address.</td></tr><tr><td><code class="language-plaintext highlighter-rouge">-y &lt;hmac-sha256:keyname:secret&gt;</code></td><td>Uses a TSIG key for authenticated requests (e.g., <code class="language-plaintext highlighter-rouge">-y hmac-sha256:keyname:secret example.com</code>).</td></tr><tr><td><code class="language-plaintext highlighter-rouge">-4</code></td><td>Forces queries over IPv4.</td></tr><tr><td><code class="language-plaintext highlighter-rouge">-6</code></td><td>Forces queries over IPv6.</td></tr><tr><td><code class="language-plaintext highlighter-rouge">@&lt;server&gt;</code></td><td>Specifies the DNS server to query (e.g., <code class="language-plaintext highlighter-rouge">@8.8.8.8</code> for Google’s public DNS).</td></tr><tr><td><code class="language-plaintext highlighter-rouge">-b &lt;address&gt;</code></td><td>Sets the source IP address for the query.</td></tr><tr><td><code class="language-plaintext highlighter-rouge">-c &lt;class&gt;</code></td><td>Specifies the query class (default is <code class="language-plaintext highlighter-rouge">IN</code>; others include <code class="language-plaintext highlighter-rouge">CH</code> and <code class="language-plaintext highlighter-rouge">HS</code>).</td></tr><tr><td><code class="language-plaintext highlighter-rouge">-h</code></td><td>Displays the help message and usage information.</td></tr><tr><td><code class="language-plaintext highlighter-rouge">-k &lt;keyfile&gt;</code></td><td>Specifies a TSIG key file for authenticated requests.</td></tr><tr><td><code class="language-plaintext highlighter-rouge">-m</code></td><td>Enables memory usage debugging.</td></tr><tr><td><code class="language-plaintext highlighter-rouge">-p &lt;port&gt;</code></td><td>Specifies the port number to query on the DNS server (default is 53).</td></tr><tr><td><code class="language-plaintext highlighter-rouge">-q &lt;name&gt;</code></td><td>Specifies the domain name or ip address to query (alternative to providing it as an argument).</td></tr><tr><td><code class="language-plaintext highlighter-rouge">-u</code></td><td>Prints the query time in microseconds instead of milliseconds.</td></tr><tr><td><code class="language-plaintext highlighter-rouge">-v</code></td><td>Displays the version of <code class="language-plaintext highlighter-rouge">dig</code>.</td></tr></tbody></table>

<table><thead><tr><th>Option</th><th>Description</th></tr></thead><tbody><tr><td><code class="language-plaintext highlighter-rouge">+additional</code></td><td>Displays only the <a href="#additional">Additional</a> section of the response.</td></tr><tr><td><code class="language-plaintext highlighter-rouge">+answer</code></td><td>Displays only the <a href="#answer">Answer</a> section of the response.</td></tr><tr><td><code class="language-plaintext highlighter-rouge">+authority</code></td><td>Shows only the <a href="#authority">Authority</a> section of the response.</td></tr><tr><td><code class="language-plaintext highlighter-rouge">+bufsize=&lt;size&gt;</code></td><td>Sets the UDP buffer size for the query.</td></tr><tr><td><code class="language-plaintext highlighter-rouge">+cookie</code></td><td>Requests a DNS Cookie for the query.</td></tr><tr><td><code class="language-plaintext highlighter-rouge">+dnssec</code></td><td>Requests DNSSEC (DNS Security Extensions) records in the response.</td></tr><tr><td><code class="language-plaintext highlighter-rouge">+edns=&lt;version&gt;</code></td><td>Enables EDNS (Extension Mechanisms for DNS) with the specified version.</td></tr><tr><td><code class="language-plaintext highlighter-rouge">+expire</code></td><td>Sends an EDNS Expire option to the server.</td></tr><tr><td><code class="language-plaintext highlighter-rouge">+multiline</code></td><td>Formats the output in a more readable, multi-line format.</td></tr><tr><td><code class="language-plaintext highlighter-rouge">+noall</code></td><td>Disables all sections of the output by default, allowing selective enabling.</td></tr><tr><td><code class="language-plaintext highlighter-rouge">+nocmd</code></td><td>Hides the initial command and version information in the output.</td></tr><tr><td><code class="language-plaintext highlighter-rouge">+nocomments</code></td><td>Removes comments and section headers from the output.</td></tr><tr><td><code class="language-plaintext highlighter-rouge">+noedns</code></td><td>Disables EDNS for the query.</td></tr><tr><td><code class="language-plaintext highlighter-rouge">+notcp</code></td><td>Forces <code class="language-plaintext highlighter-rouge">dig</code> to use UDP (default behavior).</td></tr><tr><td><code class="language-plaintext highlighter-rouge">+nsid</code></td><td>Requests the Name Server Identifier (NSID) from the DNS server.</td></tr><tr><td><code class="language-plaintext highlighter-rouge">+qr</code></td><td>Shows the query as it was sent to the DNS server.</td></tr><tr><td><code class="language-plaintext highlighter-rouge">+question</code></td><td>Displays only the <a href="#question">Question</a> section of the response.</td></tr><tr><td><code class="language-plaintext highlighter-rouge">+retry=&lt;attempts&gt;</code></td><td>Specifies the number of retries if the query fails.</td></tr><tr><td><code class="language-plaintext highlighter-rouge">+short</code></td><td>Provides a concise output, showing only essential information (e.g., IP addresses).</td></tr><tr><td><code class="language-plaintext highlighter-rouge">+stats</code></td><td>Provides statistics about the query (e.g., query time, server response).</td></tr><tr><td><code class="language-plaintext highlighter-rouge">+subnet=&lt;address&gt;</code></td><td>Sends an EDNS Client Subnet option with the specified IP address.</td></tr><tr><td><code class="language-plaintext highlighter-rouge">+tcp</code></td><td>Forces <code class="language-plaintext highlighter-rouge">dig</code> to use TCP instead of UDP for the query.</td></tr><tr><td><code class="language-plaintext highlighter-rouge">+time=&lt;timeout&gt;</code></td><td>Sets the timeout for the query in seconds.</td></tr><tr><td><code class="language-plaintext highlighter-rouge">+trace</code></td><td>Performs a trace of the DNS query, showing each step in the resolution process.</td></tr><tr><td><code class="language-plaintext highlighter-rouge">+ttlid</code></td><td>Displays the Time-to-Live (TTL) values for records in the output.</td></tr></tbody></table>

## `dig` Command you must know

1. Query Default resolver

```bash
dig exmaple.com
```

2. Query records

```bash
dig example.com A # A record

dig example.com AAAA # AAAA record

dig example.com CNAME # CNAME record

dig example.com MX

dig example.com TXT
```

3. Query a Specific Resolver

```bash
dig @8.8.8.8 example.com

dig example.com
dig @8.8.8.8 example.com
dig @1.1.1.1 example.com
```

4. Reverse DNS lookup

```bash
dig -x 8.8.8.8
```

5. **_TRACE FULL DNS PATH_**

```bash
dig +trace example.com
```

This shows the path from root DNS -> TLD -> authoritative DNS.

6. Useful Short Outputs

```bash
dig +short example.com # For clean answer

dig +short @8.8.8.8 example.com # short and clean answer with specific resolver

dig +noall +answer example.com # show only answer section
```

7. **ADVANCE QUERIES**
   Advanced queries involve more complex operations, such as querying multiple DNS servers simultaneously, performing zone transfers (although this should be done with caution), and using DNSSEC-related queries.

```bash
for server in 8.8.8.8 1.1.1.1; do dig @$server example.com; done

# This script queires both Google's public DNS server and cloudflare public DNS server for the DNS records of example.com
```

---
