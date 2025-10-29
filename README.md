```mermaid
flowchart TD
    %% Session 1 - Website
    A1[Session 1: Anonymous user browses gif pages] --> B1[Anonymous ID]
    B1 --> C1[Search: Family trip to Canada<br>6 nights, Feb 2026, 2 adults, 2 children]
    C1 --> D1[Profile identified:<br>Gold loyalty member<br>Family traveler<br>Planning Canada trip]
    D1 --> E1[Browse hotels]
    E1 --> F1[User leaves website]
    F1 --> G1[Audience built in Wandz - Family Traveler, Gold, Canada trip]

    %% Session 2 - App Visit
    F1 --> A2[Session 2: App Visit same day]
    A2 --> B2[User logs into Accor app]
    B2 --> C2[Previous search retrieved automatically<br>Cross-channel consistency]
    C2 --> D2[Clicks Search<br>Results ranked by Family Traveler profile]
    D2 --> E2[Sees Fairmont Hotel Macdonald]
    E2 --> F2[Clicks hotel page]
    F2 --> G2[Hotel page personalized:<br>Family-friendly images<br>Kids Club highlighted<br>Message: Trip upgrades you to PLATINUM]
    G2 --> H2[Browses but doesn’t book]
    H2 --> I2[Added to Abandoners segment in Wandz]

    %% Session 3 - Return to Website
    I2 --> A3[Session 3: Return to Website]
    A3 --> B3[Visits Fairmont Hotel Macdonald page]
    B3 --> C3[Abandoner pop-in triggered:<br>Family offer: Book 2nd room at 50% off]
    C3 --> D3[Clicks Book Now]
    D3 --> E3[Books 6 nights]
    E3 --> F3[Moves from Abandoner → Converter in Wandz]

    %% Confirmation & Upsell
    F3 --> G3[Booking Confirmation Page]
    G3 --> H3[Celebration pop-in:<br>Congratulations! GOLD → PLATINUM]
    H3 --> I3[Upsell offer:<br>Restaurant booking<br>50% off Taittinger champagne<br>CTA: Call Conciergerie now]
```
