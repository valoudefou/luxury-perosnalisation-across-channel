# LVMH – Omnichannel Customer Journey and Data Flow

[![Made by AB Tasty](https://img.shields.io/badge/Made%20with%20%E2%9D%A4%EF%B8%8F%20by-AB%20Tasty-red.svg)](https://www.abtasty.com)
[![GDPR Compliant](https://img.shields.io/badge/GDPR-Compliant-green.svg)](#)
[![Mermaid Diagrams](https://img.shields.io/badge/Visualised%20with-Mermaid-blue.svg)](https://mermaid.js.org)


## Table of Contents
1. [Overview](#overview)
2. [Flowchart – Data Flow Between Systems](#flowchart--data-flow-between-systems)
3. [Sequence Diagram – Customer Journey Timeline](#sequence-diagram--customer-journey-timeline)
4. [Notes](#notes)
5. [How to View Locally](#how-to-view-locally)


## Overview

This document illustrates the **LVMH omnichannel customer journey** and **data flow** across digital and in-store touchpoints.  
It highlights how AB Tasty, CRM systems, the mobile app and clienteling connect to deliver a consistent, GDPR-compliant experience.


## Flowchart – Data Flow Between Systems

The diagram below shows how user data travels across systems from the website visit to the in-store experience.

```mermaid
%% LVMH - Omnichannel Customer Journey (Flowchart)
flowchart TD
    A[Website Visitor] -->|Visits Gift category| B[AB Tasty]
    B -->|Generates anonymous ID and tracks interactions| C[AB Tasty tracking data]
    A -->|Clicks back in stock and enters email| D[CRM]
    B -->|Attaches Gift tag to email payload| D
    D -->|Stores email and creates unique ID| E[CRM database]
    D -->|Sends email with product recommendations| A2[User inbox]
    A2 -->|Clicks recommended product link| F[Mobile app]
    F -->|User creates account| G[App database]
    G -->|Matches via email and unique ID| E
    G -->|Sends user events and segments| H[Wandz SDK]
    E -->|Stores unique ID and Gift segment| I[CRM hub]
    I -->|Syncs unique ID cookie| J[Website login]
    J -->|Recognises user and personalises| B
    I -->|Shares user data and segment| K[In-store clienteling app]
    K -->|User identifies with QR or barcode| L[In-store interaction]
    L -->|Retrieves email, ID and segment| I

    subgraph GDPR compliance
        note1[All data stored in EU]
        note2[Consent managed per GDPR]
        note3[Clauses defined in client contract]
    end
```


## Sequence Diagram – Customer Journey Timeline

This sequence diagram follows the chronological customer journey and data exchanges between systems.

```mermaid
%% LVMH - Omnichannel Customer Journey (Sequence)
sequenceDiagram
    participant User
    participant Website
    participant AB_Tasty
    participant CRM
    participant Email_Svc
    participant Mobile_App
    participant Wandz_SDK
    participant Store_App

    %% Step 1
    User->>Website: Visits site and selects Gift
    Website->>AB_Tasty: Generates anonymous ID and tracks interactions
    AB_Tasty->>User: Displays product recommendations

    %% Step 2
    User->>Website: Clicks back in stock and enters email
    Website->>AB_Tasty: Tracks action, no email stored
    AB_Tasty->>CRM: Sends payload with email and Gift tag

    %% Step 3
    CRM->>CRM: Stores email and generates unique ID
    CRM->>Email_Svc: Sends unavailable product update with recommendations
    User->>Email_Svc: Clicks alternative product link

    %% Step 4
    User->>Mobile_App: Lands on app and creates account
    Mobile_App->>CRM: Matches user via email and unique ID
    Mobile_App->>Wandz_SDK: Sends events and segments
    Note right of Wandz_SDK: Data stored in EU\nConsent managed per GDPR

    %% Step 5
    User->>Website: Logs in later
    Website->>AB_Tasty: Reads cookie with unique ID
    AB_Tasty->>CRM: Syncs data for personalisation

    %% Step 6
    User->>Store_App: Identifies with QR or barcode
    Store_App->>CRM: Retrieves profile with email, ID, segment
    CRM->>Store_App: Sends personalised data
```


## Notes
- **AB Tasty** manages anonymous tracking, product recommendations and cross-channel ID linking.  
- **CRM** is the central hub connecting website, app and store.  
- **Wandz SDK** gathers in-app events and segments for personalisation.  
- **GDPR compliance** ensures data remains within the EU with explicit consent management.
