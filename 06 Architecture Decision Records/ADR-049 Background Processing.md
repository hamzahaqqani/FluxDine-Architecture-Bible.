# ADR-049 — Background Processing

## Status
Accepted

## Decision

Long-running, retryable, scheduled, or asynchronous work shall be executed through background processing rather than blocking synchronous API requests.

## Examples

- Notifications
- Email
- Analytics aggregation
- Reconciliation
- Scheduled lifecycle transitions
- Maintenance tasks


