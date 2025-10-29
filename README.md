# LVMH – Omnichannel Customer Journey and Data Flow

[![Made with ❤️ by AB Tasty](https://img.shields.io/badge/Made%20with%20%E2%9D%A4%EF%B8%8F%20by-AB%20Tasty-red.svg)](https://www.abtasty.com)
[![GDPR Compliant](https://img.shields.io/badge/GDPR-Compliant-green.svg)](#)
[![Mermaid Diagrams](https://img.shields.io/badge/Visualised%20with-Mermaid-blue.svg)](https://mermaid.js.org)

---

## Table of Contents
1. [Overview](#overview)
2. [Flowchart – Data Flow Between Systems](#flowchart--data-flow-between-systems)
3. [Sequence Diagram – Customer Journey Timeline](#sequence-diagram--customer-journey-timeline)
4. [Notes](#notes)
5. [How to View Locally](#how-to-view-locally)

---

## Overview

This documentation outlines the **end-to-end customer journey and data flow** across LVMH’s digital and physical environments.  
It highlights how AB Tasty, CRM systems, the mobile app and in-store clienteling connect to create a unified, GDPR-compliant experience.

---

## Flowchart – Data Flow Between Systems

The diagram below visualises how data moves across touchpoints, from **website visit** to **in-store interaction**, with GDPR controls included.

```mermaid
%% LVMH - Omnichannel Customer Journey (Flowchart)
flowchart TD
    A[Website Visitor] -->|Visits Gift Category| B[AB Tasty]
    B -->|Generates Anonymous ID<br>Tracks interactions| C[AB Tasty Tracking Data]
    A -->|Clicks "Back in Stock" + Enters Email| D[CRM]
    B -->|Attaches "Gift" tag to email payload| D
    D -->|Stores Email + Creates Unique ID| E[CRM Database]
    D -->|Sends Email with Product Recommendations| A2[User Email Inbox]
    A2 -->|Clicks Recommended Product Link| F[Mobile App]
    F -->|User Creates Account| G[App Database]
    G -->|Matches via Email + Unique ID| E
    G -->|Sends User Events + Segments| H[Wandz SDK]
    E -->|Stores Unique ID + Gift Segment| I[CRM Hub]
    I -->|Syncs Unique ID Cookie| J[Website Login]
    J -->|Recognises User + Personalises| B
    I -->|Shares User Data + Segment| K[In-Store Clienteling App]
    K -->|User Identifies with QR/Barcode| L[In-Store Interaction]
    L -->|Retrieves Email + ID + Segment| I

    subgraph GDPR_Compliance
        note1[All data stored in EU]
        note2[Consent managed per GDPR]
        note3[Clauses defined in client contract]
    end
