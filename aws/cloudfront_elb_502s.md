# Cloudfront ELB 502s

We were experiencing sporadic 502 between ELBs when calling from Cloudfront. It
was infrequent, maybe 20 or so calls over an hour, but added noise to our error
graphs. After weeks of of hunting, reading, and debugging, we finally got found
the issue.

It seems that it's a combination of HTTP KeepAlive and ELB Idle Timeouts. When
the HTTP KeepAlive of our services were smaller than or equal to the ELB Idle
Timeout there were a small possibility for a call to be made from Cloudfront
to a connection that was being closed by the KeepAlive timeout at the same time
due to latency. Simply lowering the ELB Idle Timeout, or increasing KeepAlive
timeout in our services (what we decided to do) seems to have fixed our issues
and the graphs and noise is back to a very quiet level again.

---
References:
* https://adamcrowder.net/posts/node-express-api-and-aws-alb-502/
