# Network analysis

## Capture metrics

| Metric | Phishing_1 | Phishing_2 |
| --- | ---: | ---: |
| Total packets | 341 | 3,797 |
| TCP packets exchanged with observed destination | 32 | 118 |

The analysis VM was `172.20.50.114`; the remote IP observed during testing was `198[.]52[.]247[.]66`.

## DNS

The captures show queries for `santander-pt[.]urgencia[.]pt` to resolver `1.1.1.1`. The first capture also contained a query for `santander-pt[.]urgencia[.]pt.internal` that returned NXDOMAIN. A local DNS search suffix may explain this; it is not independently a malicious indicator.

## TCP and TLS

TCP communication and TLS handshake records were observed with the remote address. HTTPS application content remains encrypted in the PCAPs. A separate Windows curl test reported a certificate validation error; the full chain and exact root cause were not conclusively established.

## Visible HTTP

In the second capture, visible HTTP responses included `200 OK` and HTML/JavaScript redirecting to the HTTPS version of the domain after approximately two seconds. A cookie named `pass` associated with a public IP value was observed; its value is intentionally omitted. A request for `/favicon.ico` received `404 Not Found`.

Three HTTP GET requests and no HTTP POST requests were identified in the legible traffic reviewed. This **does not rule out** activity within encrypted HTTPS sessions.

## Browser behaviour

Firefox displayed “Deceptive site ahead”. The controlled visit subsequently returned `403 Forbidden`, without a visible Santander-themed login page.

## Analytical limits

- **Observed:** DNS resolution, TCP/TLS traffic, HTTP redirect, browser warning and 403 response.
- **Not confirmed:** Working credential collection form, credential submission or exfiltration.
- **Unknown:** Why access returned 403, the role of the observed cookie and the full TLS certificate chain.

Raw packet captures are withheld from this public repository pending a privacy review.
