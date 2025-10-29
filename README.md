# Omnichannel Customer Journey and Data Flow


## Table of Contents
1. [Overview](#overview)
2. [Flowchart – Data Flow Between Systems](#flowchart--data-flow-between-systems)
3. [Sequence Diagram – Customer Journey Timeline](#sequence-diagram--customer-journey-timeline)
4. [Notes](#notes)
5. [How to View Locally](#how-to-view-locally)


## Overview

This document illustrates the **Omnichannel customer journey** and **data flow** across digital and in-store touchpoints.  
It highlights how AB Tasty, CRM systems, the mobile app and clienteling connect to deliver a consistent, GDPR-compliant experience.


## Flowchart – Data Flow Between Systems

The diagram below shows how user data travels across systems from the website visit to the in-store experience.

```mermaid
flowchart LR
  classDef hub fill:#eef,stroke:#77a,stroke-width:1px;
  classDef gdpr fill:#efe,stroke:#7a7,stroke-width:1px;
  classDef key fill:#ffe,stroke:#aa7,stroke-width:1px;

  %% Step 1 - Website visit
  subgraph S1[Step 1 • Website visit]
    A1[User visits site • selects Gift]
    A2[AB Tasty creates anon ID]
    A3[User views product]
    A4{Out of stock?}
    A5[Show similar products carousel]
    A6[Track views • clicks • filters]
    A1 --> A2 --> A3 --> A4
    A4 -- Yes --> A5 --> A6
    A4 -- No --> A6
  end

  %% Step 2 - Back in stock alert
  subgraph S2[Step 2 • Back in stock alert]
    B1[User ignores recs]
    B2[Clicks back in stock alert]
    B3[Enters email]
    B4[AB Tasty logs confirmation only]
    B5[Attach Gift tag to email payload]
    B1 --> B2 --> B3 --> B4 --> B5
  end

  %% Step 3 - CRM identification and email
  subgraph S3[Step 3 • CRM identification and email]
    C1[CRM stores email • sets unique ID]
    C2[User in Gift segment]
    C3[Later email • product still unavailable + recs]
    C4[User clicks a recommended link]
    C1 --> C2 --> C3 --> C4
  end

  %% Step 4 - Mobile app interaction
  subgraph S4[Step 4 • Mobile app interaction]
    D1[User lands in mobile app]
    D2[Creates account]
    D3[Match via email + unique ID]
    D4[Wandz SDK links events to unique ID]
    D1 --> D2 --> D3 --> D4
  end

  %% GDPR notes
  subgraph G[GDPR]
    direction TB
    G1[Data stored in EU]
    G2[Consent managed]
    G3[Legal clauses in contract]
  end
  class G,G1,G2,G3 gdpr

  %% Step 5 - Web login
  subgraph S5[Step 5 • Web login]
    E1[User logs in on website]
    E2[AB Tasty recognises user]
    E3[CRM unique ID available via cookie]
    E4[Cross channel personalisation]
    E1 --> E2 --> E3 --> E4
  end

  %% Step 6 - In store
  subgraph S6[Step 6 • In store]
    F1[User visits store • clienteling app]
    F2[Identifies via QR or barcode]
    F3[System fetches email • ID • segment • history]
    F1 --> F2 --> F3
  end

  %% CRM hub and keys
  H[(CRM hub)]
  class H hub
  K[Keys • Email • Unique ID • Segment Gift • QR or barcode • Account info]
  class K key

  %% Main flows
  A5 --> B1
  B5 --> C1
  C4 --> D1
  D3 --> H
  E3 --> H
  F3 --> H
  H --- K
  G --- H
```


## Sequence Diagram – Customer Journey Timeline

This sequence diagram follows the chronological customer journey and data exchanges between systems.

```mermaid
%% Omnichannel Customer Journey (Sequence)
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

## Text Specifications

## Step 1 – Website Visit and Product Interaction
- An anonymous user visits the website and selects the **Gift** category.  
- AB Tasty generates an **anonymous ID** for the user.  
- The user applies filters and views an **out-of-stock product**.  
- AB Tasty displays a **carousel of similar products** (same category, similar price, tagged “gift”).  
- All user interactions (**page views, clicks, filters**) are tracked by AB Tasty.


## Step 2 – Back-in-Stock Alert
- The user ignores recommendations.  
- They click **“Send me an alert when back in stock”** and enter their email.  
- AB Tasty tracks the confirmation action but **does not store the email** (GDPR compliance).  

**Technical detail:**  
- AB Tasty attaches the **“Gift” segment tag** to the email payload before sending to CRM.


## Step 3 – CRM Identification and Email Trigger
- The **CRM** stores the submitted email and assigns a **unique ID** to the user.  
- The user is categorised in the **“Gift” segment**.  
- Later, the CRM sends an **email notification** that the product is still unavailable.  
- The email includes **AB Tasty product recommendations** (similar to Step 1).  
- The user clicks a **recommended product link** in the email.


## Step 4 – Mobile App Interaction
- The user lands in the **mobile app** after clicking the email link.  
- They **create an account** in the app.  
- The app and CRM **match the user** using:  
  - The **email** from Step 2.  
  - The **unique ID** from Step 3, attached through state, variable, or local storage.  
- The **Wandz SDK** collects events and segments, linking them to the **unique ID**.  

**GDPR requirements:**  
- Data stored only in the **EU**.  
- User **consent** managed per GDPR.  
- Legal **clauses** defined in client contract.



## Step 5 – Web Login
- When the user logs in on the website, **AB Tasty recognises** them.  
- The **unique ID** from the CRM is made available via a **cookie**.  
- Enables **cross-channel personalisation** between web and app.



## Step 6 – In-Store Interaction
- The user visits a **physical store** using a **clienteling app** connected to the CRM.  
- They identify themselves using a **QR code or barcode**.  
- The system retrieves their **email, ID, segment, and history**.



## Database and CRM Implications
**Key identifiers across all systems:**
- Email address  
- Unique ID  
- Segment tag (e.g. “Gift”)  
- QR code / barcode  
- Account information  

**CRM role:**  
Acts as the **central hub** connecting the **website**, **mobile app**, **AB Tasty/Wandz**, and **store interactions**.



## Development Requirements (Implementation)
- Deploy **AB Tasty web JavaScript tag**.  
- Implement **product recommendation strategies** on web and email.  
- Store **unique ID** linked to CRM email in:  
  - Web **cookie**  
  - App **state, variable, or local storage**  
- Ensure **“Gift” category tag** and **email payload** are transmitted correctly to CRM.  
- Generate and maintain **unique IDs** in the CRM.



## Notes
- **AB Tasty** manages anonymous tracking, product recommendations and cross-channel ID linking.  
- **CRM** is the central hub connecting website, app and store.  
- **Wandz SDK** gathers in-app events and segments for personalisation.  
- **GDPR compliance** ensures data remains within the EU with explicit consent management.
