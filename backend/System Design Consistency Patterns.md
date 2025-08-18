# System Design: Consistency Patterns

With multiple copies of the same data, we are faced with options on how to synchronize them so clients have a consistent view of the data.

---
## Weak Consistency

After a write, reads may or may not see it. A best effort approach is taken.

This approach is seen in systems such as memcached. Weak consistency works well in real-time use cases such as VoIP, video chat, and realtime mutliplayer games. 

### Example

For example, if you are on a phone call and you lose connection for a few seconds, when you regain connection you do not want to hear what was spoken during connection loss.

---
## Eventual Consistency

After a write, reads will eventually see it (typically within milliseconds). Data is replicated asynchronously.

*This approach is seen in systems such as DNS and email. Eventual consistency works well in highly available systems.*

---
## Strong Consistency

After a write, reads will see it. Data is replicated synchronously.

*This approach is seen in file systems and RDBMSes. Strong consistency works well in systems that needs transactions.*

---

#backend