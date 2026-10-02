# Infra Casebook

![lint](https://github.com/DenzzarI/infra-casebook/actions/workflows/lint.yml/badge.svg)

A weekly journal of infrastructure cases: networking, automation,
security and observability. Each case solves one practical problem
in a home lab and documents how the solution was verified.

## Lab

- **Hypervisor:** Proxmox VE on a Dell PowerEdge R640
- **Scenario:** a fictional small company with several offices and
  one server room. All names, addresses and data are invented.

## Cases

| #  | Case | Stack | Status |
|----|------|-------|--------|
| 01 | CI for this repository | GitHub Actions, yamllint, gitleaks | ✅ Done |

## Case format

Every case follows the same structure (see [`_template/CASE.md`](_template/CASE.md)):
problem → context → solution → verification → lessons learned.
