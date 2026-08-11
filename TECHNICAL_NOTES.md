# Technical Notes

These are the parts of the research I considered worth keeping in the public repository. I left out raw dumps, live tokens and anything that would add risk without adding much technical value.

## TLS and certificate pinning

The first stage was establishing a clean baseline. Proxy connections were visible, but useful application traffic was not consistently available and some connections terminated before the application-layer exchange could be inspected.

I then worked through the certificate / networking path in a controlled Android test environment and compared the behavior before and after each change. The important result was not simply seeing a successful CONNECT request; it was reaching the point where the actual HTTP2 request and decoded response were visible.

The screenshots in this repository show that progression.

## GraphQL traffic

Once the traffic was observable, I followed GraphQL requests from Meta Business Suite and compared request metadata with the returned data.

One captured flow used the friendly name `BizAppHomeGenericNativeTemplateViewQuery`. The request travelled over HTTP2 and the response was gzip-compressed before being decoded by the inspection tool.

I also paid attention to the networking metadata around these requests. Headers in the captured traffic exposed the **Tigon/Liger** networking path, which was useful when correlating the network trace with native Android analysis.

## Native networking

Static and runtime analysis were used together rather than trusting a single decompiler view. I followed references and call paths around the application/native boundary, then checked the assumptions against runtime behavior and captured network traffic.

That made it easier to distinguish three different problems that can otherwise look similar from the outside:

1. normal proxy configuration issues;
2. certificate trust / TLS validation failures;
3. application-specific protection and native networking behavior.

## `#PWD_ENC:2`

A separate part of the project looked at the client-side password envelope used by the application. I mapped the structure at a protocol level, including the authenticated symmetric-encryption step, public-key wrapping of the generated key, the key identifier, timestamp and final serialized envelope.

The public repository intentionally stops at the research description. I am not publishing live credentials, captured passwords or a production-ready authentication tool.

## Publication boundary

The screenshots are sanitized copies. Authentication values, device/session identifiers and account-specific values were removed where they were not needed to understand the result.

This repository documents a research process. It is not a vulnerability disclosure unless a specific security flaw is separately demonstrated and reported through the appropriate channel.
