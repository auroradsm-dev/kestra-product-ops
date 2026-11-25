# Decisions

## 001: Rename the repo to kestra-product-ops
Date: 25 Nov 2026
Decision: rename kestra-app-monitor to kestra-product-ops.
Why: this is not only a monitoring feature anymore. It includes product
workflows and automation. I am building the DevOps and Product team I don't have.

## 002: How I use AI on this project
Date: 25 Nov 2026
Decision: I have used AI to help me grasp concepts I am not familiar with.
If something is unclear to me, the LLM explains it in as many steps as I need.
I make all the decisions about what to build and how. AI helps me execute
while I learn along the way. I am not an engineer: Claude acts as my code
reviewer and sometimes tech lead.

## 003: Supabase guard runs every 6 hours, not every hour
Date: 25 Nov 2026
Decision: ping every 6 hours.
Why: the product is in production but has no traffic yet, so hourly is
overkill for now, and hourly could mean an alert every hour during an outage.
It is easy to change when needed.
Trade off I accept: a pause could go unnoticed for up to 6 hours.
With no users yet, that affects nobody.
What would change my mind: the first real users.

## 004: A heartbeat gap is not proof of downtime
Date: 25 Nov 2026
The alert assumed the database was down. In fact the guard itself wasn't
running: I had only run it locally on my Mac. The database was fine all along.
Lesson: the alert measures missed pings, not real downtime.

## 005: One Kestra instance for everything
Date: 25 Nov 2026
Decision: run everything on one Kestra instance with Postgres.
Why: it all shares the same plumbing. Same catalog, same database,
same sync from GitHub.

## 006: CI validates flows against a throwaway Kestra
Date: 25 Nov 2026
Decision: on each push, CI starts a temporary Kestra, validates every flow
against it, then throws it away.
Options I considered: wait for my own server (week 2), or a basic YAML check only.
Why: fully self contained, no server exposed.
Trade off I accept: each run downloads the full Kestra image and takes a few minutes.

## 007: Geekasong checks: 1500 ms limit, and a health check room
Date: 25 Nov 2026
Decision: site and rooms checks both allow 1500 ms.
Why: measured about 70 ms for the site and 120 ms for a room. 1500 ms leaves
room for slow networks without hiding a real problem.
Side effect I accept: each check wakes a room called kestra-healthcheck.
No player sees it and it goes back to sleep.
Lesson: I checked each page's real title before using it as expected text.
Mypetravel's title is "My Pet Travel", so a guess would have alerted on a healthy site.
