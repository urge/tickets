ID | Summary | Detailed Acceptance Criteria | Owner | Story Points | Sprint Goal Link | Demo Artifact
---|---------|-----------------------------|-------|--------------|------------------|--------------
[EPIC‑A] ID01 | Create Okta Kinesis stream | **Given** Okta syslog API creds<br>**When** Terraform applies<br>**Then** new Kinesis stream `okta_raw` receives events within 5 s (§data‑flow Fig 1) | DataEng | 3 | Sprint 1 | CloudWatch metric screenshot
[EPIC‑A] ID02 | Parse Okta JSON to unified schema | Given raw Okta event<br>When Lambda `parser_okta` runs<br>Then output matches Table 5.1 fields 1‑8 (§event‑schema) | DataEng | 5 | Sprint 1 | CLI `jq` diff
[EPIC‑A] ID03 | Tier‑1 validation enforcement | Given event missing `user_id`<br>When validation executes<br>Then record quarantined & `tier1_fail` metric++ (§validation‑rules) | DataEng | 2 | Sprint 1 | Quarantine log
[EPIC‑A] ID04 | Duplicate‑event suppression | Given duplicate `event_id` in 24 h<br>When pipeline runs<br>Then only first stored (§validation‑rules) | DataEng | 3 | Sprint 2 | QLDB query
[EPIC‑A] ID05 | Ping feed connector | Given Ping token<br>When forwarder deployed<br>Then 100 % Ping logs land in `ping_raw` (§data‑flow) | DataEng | 5 | Sprint 2 | Grafana ingestion panel
[EPIC‑A] ID06 | AD audit‑log ingestion via EventBridge | Given AD logs<br>When rule enabled<br>Then events stream to S3 bronze (§data‑flow) | Infra | 8 | Sprint 2 | EventBridge JSON
[EPIC‑A] ID07 | Medallion S3 bucket setup | Given naming standard<br>When TF plan applied<br>Then bronze/silver/gold buckets exist w/ SSE‑KMS (§data‑flow) | Infra | 2 | Sprint 1 | S3 console screenshot
[EPIC‑A] ID08 | IAM least‑priv for parser Lambda | Given policy doc<br>When Lambda invoked<br>Then only `s3:PutObject` silver allowed (§security‑itdr) | SecEng | 3 | Sprint 2 | IAM access‑advisor report
[EPIC‑A] ID09 | CloudWatch ingest latency alert | Given Kinesis `MillisBehind` > 5 s<br>When threshold breached<br>Then PagerDuty Sev‑2 fires (§slas‑slos) | SecOps | 2 | Sprint 3 | PD alert demo
[EPIC‑A] ID10 | Validation unit tests | Given edge‑case fixtures<br>When pytest run<br>Then all Tier‑1/2 rules pass (§validation‑rules) | QA | 3 | Sprint 3 | CI green badge
[EPIC‑A] ID11 | Terraform CI pipeline | Given PR to `main`<br>When workflow runs<br>Then security scan passes (§deployment‑mlops) | DevOps | 5 | Sprint 3 | GitHub Actions log
[EPIC‑A] ID12 | Data‑quality dashboard | Given Athena query<br>When daily job runs<br>Then Tier‑1 fail % plotted (§slas‑slos) | BI | 8 | Sprint 4 | Grafana panel
[EPIC‑A] ID13 | Geo‑IP enrichment micro‑service | Given IP list<br>When enricher called<br>Then adds `location` field w/ ISO cc (§feature‑engineering) | DataEng | 5 | Sprint 4 | Postman demo
[EPIC‑A] ID14 | Source‑feed health checker | Given heartbeat absent 5 m<br>When Lambda runs<br>Then Slack alert to #id‑feed (§monitoring‑compliance) | SecOps | 2 | Sprint 4 | Slack screenshot
[EPIC‑A] ID15 | Bronze→Silver ETL Glue job | Given raw S3<br>When Glue crawler completes<br>Then validated parquet in silver (§data‑flow) | DataEng | 8 | Sprint 4 | Glue job log
[EPIC‑B] ID16 | Feature‑enrichment service skeleton | Given HR CSV<br>When Lambda `enricher` runs<br>Then adds dept & manager fields (§feature‑engineering) | DataEng | 3 | Sprint 2 | Unit‑test log
[EPIC‑B] ID17 | RCF model training notebook | Given 4‑week data<br>When executed<br>Then model tar saved (§model‑training) | DataSci | 5 | Sprint 2 | Jupyter viewer
[EPIC‑B] ID18 | Deploy RCF endpoint (canary) | Given model tar<br>When SM deploy<br>Then inference < 200 ms (§deployment‑mlops) | DataSci | 3 | Sprint 3 | Postman GIF
[EPIC‑B] ID19 | Isolation Forest batch job | Given dataset<br>When IF training completes<br>Then importance JSON saved (§ml‑model‑selection) | DataSci | 5 | Sprint 3 | Artifact JSON
[EPIC‑B] ID20 | Ensemble risk‑score lambda | Given RCF+IF scores<br>When invoked<br>Then outputs `risk_level` (§technical‑architecture) | DataSci | 3 | Sprint 3 | Lambda test
[EPIC‑B] ID21 | Add `has_*` missing flags | Given parser output<br>When enrichment runs<br>Then missing indicators set (§sparse‑data) | DataEng | 2 | Sprint 4 | Feature diff
[EPIC‑B] ID22 | DeepAR training pipeline | Given 6‑mo series<br>When SM batch<br>Then accuracy ≥ 80 % (§model‑training) | DataSci | 8 | Sprint 4 | Validation plot
[EPIC‑B] ID23 | Drift‑monitor config | Given RCF endpoint<br>When dist shifts >3σ<br>Then CW alarm fires (§monitoring‑compliance) | SecEng | 3 | Sprint 4 | Alarm demo
[EPIC‑B] ID24 | Anomaly threshold store | Given JSON<br>When parameter changed<br>Then StepFn reads new value (§ml‑model‑selection) | DevOps | 2 | Sprint 4 | Parameter screenshot
[EPIC‑B] ID25 | Blue/green SM switch script | Given canary pass<br>When script runs<br>Then 100 % traffic to green (§deployment‑mlops) | DevOps | 5 | Sprint 4 | CodePipeline demo
[EPIC‑B] ID26 | Analyst feedback UI stub | Given alert list<br>When FP tagged<br>Then record in Dynamo `feedback` (§monitoring‑compliance) | UX Dev | 8 | Sprint 5 | React demo
[EPIC‑B] ID27 | Auto‑threshold tuner cron | Given 30‑d labels<br>When cron runs<br>Then threshold optimised (F1) (§monitoring‑compliance) | DataSci | 13 | Sprint 6 | CLI report
[EPIC‑B] ID28 | Model registry Terraform | Given ECR path<br>When TF apply<br>Then SM Model Registry entry (§deployment‑mlops) | DevOps | 3 | Sprint 5 | TF plan
[EPIC‑B] ID29 | Explainability SHAP script | Given IF model<br>When run<br>Then top features CSV (§ml‑model‑selection) | DataSci | 5 | Sprint 5 | CSV output
[EPIC‑B] ID30 | Weekly retrain scheduler | Given cron<br>When time reached<br>Then StepFn triggers SM job (§model‑training) | DevOps | 3 | Sprint 5 | CW Events detail
[EPIC‑C] ID31 | High‑risk alert to SIEM | Given risk=HIGH<br>When StepFn reaches node<br>Then JSON posted to Splunk HEC (§response) | SecOps | 3 | Sprint 3 | Splunk event
[EPIC‑C] ID32 | Auto‑MFA challenge Lambda | Given impossible‑travel<br>When policy hit<br>Then Cognito prompts MFA (§response) | SecEng | 5 | Sprint 3 | Auth flow video
[EPIC‑C] ID33 | Account disable action | Given crit risk + policy<br>When lambda executed<br>Then AD user disabled < 60 s (§slas‑slos) | SecEng | 3 | Sprint 3 | AD before/after
[EPIC‑C] ID34 | QLDB ledger writer | Given scored event<br>When StepFn ends<br>Then record immutably stored (§audit‑ledger) | DataEng | 5 | Sprint 2 | QLDB query
[EPIC‑C] ID35 | Ledger integrity digest job | Given nightly schedule<br>When job runs<br>Then Merkle root saved (§compliance‑map) | DataEng | 3 | Sprint 4 | Digest file
[EPIC‑C] ID36 | Tier‑3 suspicious‑string rule | Given `<script>` in data<br>When regex fires<br>Then flag `suspicious_pattern=1` (§validation‑rules) | DataEng | 2 | Sprint 3 | Unit test
[EPIC‑C] ID37 | SOC workflow runbook | Given alert playbook<br>When analyst follows doc<br>Then incident closed (§execution‑plan) | SecOps | 2 | Sprint 4 | Confluence page
[EPIC‑C] ID38 | GDPR breach flag logic | Given personal‑data read + high risk<br>When Macie concur<br>Then GDPR timer starts (§compliance‑map) | Compliance | 5 | Sprint 5 | Jira incident
[EPIC‑C] ID39 | Executive KPI dashboard | Given CW metrics<br>When Grafana loads<br>Then MTTD, FP %, containment shown (§exec‑dashboard) | BI | 8 | Sprint 5 | Grafana share
[EPIC‑C] ID40 | Alert grouping logic | Given >3 anomalies same user/10 m<br>When rule runs<br>Then single incident created (§monitoring‑compliance) | SecOps | 3 | Sprint 4 | Test result
[EPIC‑C] ID41 | Auto‑unlock safe‑list | Given user tagged VIP<br>When risk resets<br>Then account unlock script (§governance‑accountability) | SecEng | 2 | Sprint 5 | Audit log
[EPIC‑C] ID42 | Monthly compliance PDF | Given ledger<br>When Athena query runs<br>Then PDF emailed (§compliance‑management) | BI | 5 | Sprint 6 | Sample PDF
[EPIC‑C] ID43 | CW metric math for FP% | Given FP & TP<br>When math alarm built<br>Then thresholds alert >5 % (§monitoring‑compliance) | SecOps | 2 | Sprint 4 | CW console
[EPIC‑C] ID44 | Response policy YAML store | Given new policy file<br>When merged<br>Then StepFn loads in next exec (§security‑governance) | DevOps | 3 | Sprint 5 | YAML diff
[EPIC‑C] ID45 | Auto SOC ticket in ServiceNow | Given HIGH alert<br>When webhook fires<br>Then SN incident created (§response) | SecOps | 3 | Sprint 5 | SN incident
[EPIC‑D] ID46 | Terraform module registry | Given core TF<br>When `make publish`<br>Then module versioned (§dev‑experience) | DevOps | 3 | Sprint 1 | Registry page
[EPIC‑D] ID47 | One‑click sandbox script | Given local Mac<br>When `./up.sh` run<br>Then Docker Compose pipeline (§dev‑experience) | DevOps | 5 | Sprint 2 | Asciicast
[EPIC‑D] ID48 | Local↔Cloud parity tests | Given sandbox output<br>When integration tests run<br>Then same results (§dev‑experience) | QA | 3 | Sprint 3 | Test matrix
[EPIC‑D] ID49 | CI/CD GitHub Actions for Lambdas | Given push<br>When workflow runs<br>Then artifact deployed (§deployment‑mlops) | DevOps | 2 | Sprint 2 | Actions log
[EPIC‑D] ID50 | Cost‑explorer tag policy | Given AWS org<br>When resource created<br>Then tag enforced `CostCenter=ITDR` (§cost‑management) | FinOps | 3 | Sprint 3 | Config rule
[EPIC‑D] ID51 | Savings‑plan analysis script | Given 30‑d usage<br>When run<br>Then RI CSV produced (§cost‑management) | FinOps | 5 | Sprint 4 | CSV file
[EPIC‑D] ID52 | Drift‑alert Slackbot | Given CW alarm<br>When fires<br>Then Slack msg to #sec‑ml (§monitoring‑compliance) | SecOps | 2 | Sprint 4 | Slack screenshot
[EPIC‑D] ID53 | Load‑test 1 k eps | Given k6 script<br>When exec<br>Then p95 latency <30 s (§slas‑slos) | QA | 5 | Sprint 5 | k6 report
[EPIC‑D] ID54 | Glacier export lifecycle | Given QLDB ≥1 yr<br>When rule active<br>Then objects to Glacier (§cost‑management) | DevOps | 5 | Sprint 5 | S3 lifecycle XML
[EPIC‑D] ID55 | Quarterly red‑team drill | Given playbook<br>When executed<br>Then ITDR detects ≥90 % steps (§continuous‑improvement) | SecOps | 8 | Sprint 6 | After‑action report
[EPIC‑D] ID56 | Upgrade runbooks v1.0 | Given prod go‑live<br>When docs updated<br>Then Confluence signed‑off (§execution‑plan) | PM | 2 | Sprint 6 | PDF export
[EPIC‑D] ID57 | Developer ADR template | Given architectural decision<br>When template used<br>Then decision logged (§dev‑experience) | DevOps | 2 | Sprint 4 | ADR markdown
[EPIC‑D] ID58 | Pre‑commit hooks repo | Given git repo<br>When commit staged<br>Then lint & secrets scan pass (§security‑itdr) | DevOps | 3 | Sprint 3 | Hook output
[EPIC‑D] ID59 | Sandbox seed‑data generator | Given HR sample<br>When script runs<br>Then 10 k synthetic events (§dev‑experience) | DataEng | 5 | Sprint 4 | CSV preview
[EPIC‑D] ID60 | Developer onboarding guide | Given new hire<br>When guide followed<br>Then local tests green (§dev‑experience) | PM | 3 | Sprint 6 | Markdown guide
