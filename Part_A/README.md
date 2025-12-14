# Part A - VAmPI Security Analysis

This document contains all the work submitted for Part A of the ITC4247 Final Project.

## Contents

All files have been pushed to the `main` branch of the repository: https://github.com/ipapidi/ITC4247_Final_Project

### Files Pushed

1. **INSTRUCTIONS.md** - Complete setup and testing instructions for running VAmPI
   - Docker setup instructions
   - Basic API testing procedures
   - Windows and macOS/Linux command examples
   - Troubleshooting guide

2. **bandit_report.json** - Static security analysis results from Bandit scanner
   - Contains security findings from automated code analysis
   - JSON format for programmatic processing

### Repository Status

- **Branch:** main (formerly master)
- **Last Commit:** "Added Part A" (ab39406)
- **Status:** All files successfully pushed to remote repository

---

## INSTRUCTIONS.md Summary

The INSTRUCTIONS.md file provides:

- **Setup Instructions:**
  - Docker Compose setup for secure and vulnerable instances
  - Single container setup option
  - Database initialization steps

- **Basic Testing:**
  - Viewing users
  - User registration
  - Login and token extraction (PowerShell and Bash methods)
  - Protected endpoint testing

- **Vulnerability Testing:**
  - Comparison between secure (port 5001) and vulnerable (port 5002) instances
  - Instructions for testing unauthorized password changes

- **Troubleshooting:**
  - Common issues and solutions
  - Platform-specific commands (Windows/Mac/Linux)
  - Token extraction troubleshooting

- **Additional Resources:**
  - Swagger UI access information
  - Default user credentials

---

## Project Overview

VAmPI is a vulnerable API built with Flask that intentionally contains multiple security vulnerabilities from the OWASP Top 10 for APIs. This Part A submission includes:

1. Complete setup and testing instructions
2. Automated security analysis results (Bandit)

All documentation is ready for use by instructors/evaluators to run and test the VAmPI application.

---

**Submitted:** Part A - ITC4247 Final Project
**Repository:** https://github.com/ipapidi/ITC4247_Final_Project
**Branch:** main

