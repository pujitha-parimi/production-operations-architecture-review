# Rollback and Canary Deployment Plan

## 1. Purpose

This document defines a safe approach for releasing changes to an agentic AI system using canary deployment, monitoring, and rollback procedures.

## 2. Deployment Strategy

Use a gradual rollout to reduce the risk of introducing failures into the production environment.

1. Validate the change in a development or staging environment.
2. Deploy the new version to a small percentage of traffic.
3. Monitor errors, latency, tool failures, and model costs.
4. Compare the canary results with the existing stable version.
5. Increase traffic gradually only when the canary meets the defined acceptance criteria.
6. Stop the rollout if significant regressions appear.

Traffic percentages and evaluation windows should be selected according to the service's risk and traffic volume.

## 3. Pre-Deployment Checklist

- [ ] Tests pass in the intended environment.
- [ ] Configuration and dependencies are reviewed.
- [ ] Secrets are stored securely and are not committed to Git.
- [ ] A known-good version is available for rollback.
- [ ] Health checks and monitoring are operational.
- [ ] Alert rules and dashboard queries are validated.
- [ ] The release owner and rollback decision-maker are identified.

## 4. Canary Evaluation Signals

| Signal | Evaluation |
|---|---|
| Error rate | Check for an increase compared with the stable version. |
| Latency | Compare p95 latency against the agreed target. |
| Tool failures | Check for unexpected increases in failed tool calls. |
| Availability | Confirm that health checks remain successful. |
| Model cost | Check for unusual token usage or estimated cost increases. |
| Output quality | Evaluate representative responses against predefined quality criteria. |

Define acceptance thresholds before deployment. Compare equivalent workloads and time windows wherever possible.

## 5. Rollout Procedure

### Step 1: Validate

Run automated tests and verify the candidate release in a safe environment.

### Step 2: Start the Canary

Route a small, controlled portion of traffic to the candidate version. Keep the stable version available.

### Step 3: Monitor

Observe the evaluation signals, inspect relevant logs, and review available traces. Continue only after the defined evaluation window and acceptance criteria are satisfied.

### Step 4: Expand Gradually

Increase the traffic share in controlled stages. Recheck the acceptance criteria after each stage.

### Step 5: Complete the Release

Route traffic to the new version only after the rollout passes the required checks. Record the release version and outcome.

## 6. Rollback Triggers

Stop the rollout and consider rollback when:

- Error rates exceed the agreed threshold.
- Latency breaches the defined objective.
- Tool failures increase unexpectedly.
- Health checks fail or availability degrades.
- Model cost rises beyond the approved budget.
- Output quality or security checks fail.

Critical security or safety failures require immediate containment according to the incident response policy.

## 7. Rollback Procedure

1. Stop further traffic increases.
2. Route traffic back to the last known-good version, where supported.
3. Restore the previous configuration if the change caused the problem.
4. Verify service health and representative requests.
5. Check that errors, latency, and tool failures return to acceptable levels.
6. Preserve relevant logs, metrics, and traces for investigation.
7. Notify stakeholders and document the rollback.

Database or data-format changes may require a separate backward-compatible recovery plan. Do not assume that reverting application code automatically reverses data changes.

## 8. Post-Deployment Verification

- [ ] Health checks succeed.
- [ ] Prometheus target health is UP.
- [ ] Error rate and latency are within agreed limits.
- [ ] Tool execution works as expected.
- [ ] Cost and token usage are reasonable.
- [ ] Representative output-quality checks pass.
- [ ] The release outcome is documented.

## 9. Limitations

This plan describes a proposed deployment process. Actual canary traffic routing and automated rollback depend on the deployment platform and supporting infrastructure. They are not provided automatically by a monitoring dashboard alone.

## 10. References

- [Tracing and Metrics Plan](tracing-metrics-plan.md)
- [Incident Response Runbook](incident-response-runbook.md)
- [Postmortem Template](postmortem-template.md)