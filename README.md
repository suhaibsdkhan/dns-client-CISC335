# dns-client

A DNS client written with raw UDP sockets for CISC 335 (Computer Networks) at Queen's, winter 2025. No DNS library: it packs the query by hand with `struct`, sends it to 8.8.8.8, and parses the reply.

```
$ python3 dns_client.py queensu.ca

;; Got answer:
;; ->>HEADER<<- opcode: 0, status: 0, id: 49586
;; QUESTIONS: 1, ANSWERS: 1, AUTHORITY: 0, ADDITIONAL: 0

;; QUESTION SECTION:
;; queensu.ca	IN	A

;; ANSWER SECTION:
queensu.ca	47	IN	A	52.139.22.141
```

Run it with no argument and it prompts for a hostname.

## What it handles

- Builds the 12-byte header (random ID, recursion desired) and the question section for an A record
- Parses the answer, authority and additional sections, including A, NS, CNAME, PTR and MX records
- Follows compression pointers in names (the `0xC0` prefix)
- Times out after 5 seconds

## Not handled

- Only asks for A records, even though it can parse the others when they come back
- UDP only, so truncated responses (the TC bit) aren't retried over TCP
- The server is hardcoded in `main()`
