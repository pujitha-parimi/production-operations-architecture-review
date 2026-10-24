# Incident Response Runbook

## 1. Purpose

This runbook defines the steps for identifying, investigating, mitigating, and reviewing incidents affecting an agentic AI system.

## 2. Incident Severity

| Severity | Example | Response Priority |
|---|---|---|
| SEV-1 | Complete service outage, major security incident, or widespread failures | Immediate response and escalation |
| SEV-2 | Sustained high error rate, severe latency, or major tool failure | Urgent investigation |
| SEV-3 | Limited impact, intermittent failures, or degraded noncritical functionality | Investigate and resolve during normal operations |

Severity should be adjusted according to customer impact, business risk, and the organization's incident policy.

## 3. Detection and Alerting

Monitor the following signals:

- Request error rate
- Request latency, especially p95
- Tool execution failures
- Service availability
- Model token usage and estimated cost

Use Prometheus alerts and Grafana dashboards to identify abnormal behavior. Confirm the alert and its time range before starting mitigation.

## 4. Incident Response Procedure

### Step 1: Acknowledge

- Record the incident start time and alert name.
- Identify the affected service or workflow.
- Assign an incident owner.
- Communicate the current impact to relevant stakeholders.

### Step 2: Investigate

- Check the agent health endpoint.
- Inspect Prometheus target health and relevant metrics.
- Review application logs for errors and exceptions.
- Inspect available traces if a tracing backend is configured.
- Identify recent changes, failing tools, latency increases, or unusual usage.

### Step 3: Mitigate

Choose the safest action based on the evidence:

- Roll back a recent change if it is the likely cause.
- Disable or isolate a failing optional tool when safe.
- Apply configured timeouts and bounded retries.
- Reduce traffic or disable a problematic workflow if necessary.
- Escalate security incidents to the responsible security team.

Do not make destructive changes without authorization and an appropriate recovery plan.

### Step 4: Verify Recovery

- Confirm that the health endpoint responds successfully.
- Verify that Prometheus targets are UP.
- Check whether the error rate and latency return to acceptable levels.
- Confirm that representative requests succeed.
- Continue monitoring for recurring failures.

### Step 5: Communicate and Close

- Share the impact, mitigation, and current status.
- Record the incident timeline and important decisions.
- Document the root cause when established.
- Create follow-up actions with owners and target dates.
- Close the incident after recovery is verified.

## 5. Escalation

Escalate incidents when they exceed the team's response capability, affect critical workflows, involve suspected security or data exposure, or fail to recover after initial mitigation.

Include the incident severity, impact, timestamps, relevant logs, metrics, actions taken, and current status in the escalation.

## 6. Rollback and Recovery

Follow the documented rollback and canary plan for releases. Verify configuration, dependencies, and health checks after rollback. Preserve relevant logs and telemetry for investigation.

## 7. Post-Incident Review

For significant incidents, document:

- Summary and customer impact
- Start time, detection time, and recovery time
- Root cause and contributing factors
- Detection and response effectiveness
- What went well and what needs improvement
- Preventive actions, owners, and due dates

Focus on improving the system and process rather than assigning blame.

## 8. Operational Safety

- Never include passwords, API keys, or access tokens in incident notes.
- Preserve relevant evidence when investigating security incidents.
- Follow access-control and approval requirements.
- Test recovery procedures in a safe environment before relying on them in production.

## 9. Validation Checklist

- [ ] Health endpoint checked
- [ ] Prometheus target health checked
- [ ] Relevant metrics and alerts reviewed
- [ ] Logs and available traces inspected
- [ ] Mitigation and recovery verified
- [ ] Stakeholders updated
- [ ] Follow-up actions documented