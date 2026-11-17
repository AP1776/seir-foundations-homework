# Lesson 2 Homework: Slack Alerting and Monitoring

**Student:** Alex Petit  
**Course:** SEIR Foundations  
**Lesson:** Module 1, Lesson 2 — Slack Alerts  
**Environment:** AWS EC2 (`t3.small`, Linux), accessed over SSH using Git Bash  
**Alert channel:** `#balerica-alerts`

## Overview

For this homework, I ran seven Python scripts on my AWS EC2 instance and verified that they sent operational messages to my Slack workspace. The scripts cover alert severity, UTC timestamps, disk utilization, CPU load, simulated incidents, and a combined server-health report.

I ran the scripts from `/opt/balerica-bank` using the project's Python virtual environment. Alerts were sent through the provided `send_alert()` interface in `slack.py`. I used only test or synthetic incident messages.

## Seven Homework Scripts

| # | Script | What I tested | Result |
|---|---|---|---|
| 1 | `hw1_hello_slack.py` | Personalized INFO alert containing my name | Delivered to Slack |
| 2 | `hw2_severity_tour.py` | INFO, WARNING, ERROR, and CRITICAL alert levels | All four delivered with the minimum severity set to INFO |
| 3 | `hw3_server_time.py` | UTC timestamp reported by EC2 | Delivered; compared UTC against Virginia local time |
| 4 | `hw4_disk_watch.py` | Actual root-disk utilization and severity selection | Approximately 25.6% used; INFO delivered |
| 5 | `hw5_load_check.py` | One-, five-, and fifteen-minute load averages | `0.00 / 0.00 / 0.00` across 2 virtual CPUs; INFO delivered |
| 6 | `hw6_incident_drill.py` | Three random, simulated bank incidents | All three delivered |
| 7 | `hw7_morning_report.py` | Combined timestamp, host, Python version, disk, and load report | Report delivered |

My three incident-drill results were:

- **CRITICAL:** An account-lookup operation could not reach the ledger file.
- **ERROR:** The `/payments` API returned HTTP 500 twice within one minute.
- **ERROR:** A second simulated `/payments` API HTTP 500 incident.

The final morning report showed Python `3.9.25`, approximately **25.6%** disk utilization, and a one-minute load average of **0.00**. I also ran the required identification command to display my Linux username, Virginia time, and EC2 instance ID.

## Homework Questions and Answers

### 1. Which severity levels arrived, and why?

INFO, WARNING, ERROR, and CRITICAL all arrived during the first severity test. My minimum alert threshold was set to INFO, so none of the four levels was filtered out.

### 2. How far apart were the server time and my local clock?

The EC2 script reported UTC. My local time in Virginia was Eastern Standard Time (EST), which was **five hours behind UTC** in November. UTC provides a consistent time reference when logs or services span multiple time zones.

### 3. Why is a quiet INFO disk alert good design, not a failure?

Normal disk usage does not require urgent attention. A monitor can still measure and log a healthy state without paging anyone. Filtering routine INFO messages in production reduces alert fatigue and helps engineers focus on actual problems.

### 4. What could make CPU load spike on the Balerica bank server?

Heavy transaction traffic, resource-intensive batch processing, or several competing applications could increase CPU load. A sustained increase can indicate that work is queueing faster than the server can handle it.

### 5. Which simulated incident deserves to wake someone at 3 AM?

The CRITICAL incident where the banking ledger could not be reached deserves immediate attention because core financial operations could be disrupted. Repeated payment API errors might also require an urgent response if customers cannot complete transactions.

## Configuration Experiments

### Experiment 1: Critical-only alert filtering

- **Change:** Set `SLACK_ALERT_SEVERITY=CRITICAL`.
- **Prediction:** Exactly one of the four severity messages would arrive.
- **Observed:** INFO, WARNING, and ERROR were skipped; CRITICAL was delivered.
- **Conclusion:** My prediction was correct. The minimum-severity setting filters out lower-priority alerts.

### Experiment 2: Alert kill switch

- **Change:** Set `SLACK_ALERTS_ENABLED=false` while the minimum severity was INFO.
- **Test:** Re-ran the personalized `hw1_hello_slack.py` alert.
- **Result:** The alert was skipped rather than posted to Slack.
- **Conclusion:** A kill switch helps prevent unwanted notifications during planned maintenance or testing without modifying the alerting code.

### Experiment 3: Disk-warning threshold

- **Change:** Temporarily lowered the disk WARNING threshold from 75% to 20% in `hw4_disk_watch.py`.
- **Prediction:** With disk utilization around 25.6%, the alert would change from INFO to WARNING.
- **Observed:** The script selected WARNING and delivered the alert.
- **Conclusion:** Changing a monitoring threshold changes how the same measured condition is classified.

After the experiments, I restored the original disk WARNING threshold to **75%** and restored the Slack settings to:

```dotenv
SLACK_ALERTS_ENABLED=true
SLACK_ALERT_SEVERITY=INFO
```

## Evidence

The submission includes these screenshots in the `screenshots/` folder:

1. [`slack_hw_alerts.jpg`](screenshots/slack_hw_alerts.jpg): The `#balerica-alerts` channel showing at least five different homework alerts.
2. [`hw7_morning_report.jpg`](screenshots/hw7_morning_report.jpg): The terminal output from `hw7_morning_report.py` with the delivery confirmation and the required `whoami`, Virginia date, and EC2 instance identification line.

Both screenshots should have any sensitive information hidden before submission.

## Security and Lessons Learned

- The Slack Incoming Webhook URL is a secret and stays in the server's local `.env` file. It is **not** included in this repository or in screenshots.
- The workspace URL identifies the Slack workspace; the webhook controls where messages are actually delivered.
- Alert thresholds and the enable/disable setting can change notification behavior without modifying the alert-sending function.
- The incident drill used fake events and no real customer information.
- Measuring system health, selecting a severity, and recording the result are the foundation of useful automated monitoring.

## Submission Checklist

- [x] Run all seven Python homework scripts.
- [x] Verify alerts in `#balerica-alerts`.
- [x] Complete the three configuration experiments.
- [x] Restore the original configuration and disk threshold.
- [x] Answer the homework questions.
- [x] Add both screenshots to `screenshots/` and verify their filenames match the links above.
- [x] Commit and push this README and the screenshots to my personal homework repository.

**Reference:** [Original Lesson 2 homework instructions](https://github.com/BalericaAI/SEIR-Foundations/tree/main/module1/lesson2_slack/homework)
