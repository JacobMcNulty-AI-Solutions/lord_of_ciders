# Cider Logging System

This document outlines the design of the cider logging system.

## Core Metadata Fields

### Required Fields

*   Cider name
*   Brand/Producer
*   Photo
*   Personal rating (1-10 tankards)

### Optional Fields

*   Alcohol percentage
*   Package type (bottle, can, on tap)
*   Location first tried (with GPS coordinates)
*   Social context (alone, with friends, special occasion, etc.)
*   Price paid
*   Cider style
*   Origin country/region
*   Carbonation level (still, lightly sparkling, highly carbonated)
*   Clarity (cloudy, hazy, clear, crystal clear)
*   Color description
*   Detailed tasting notes
*   Venue type (pub, home, restaurant, festival, beer garden)

## Logging Flow

*   **Quick Log Mode**: Allows for rapid logging with minimal information (photo, location, rating).
*   **Detailed Log Mode**: A comprehensive form to enter all metadata fields.

## Re-logging System

*   Users can "re-log" a cider they have had before.
*   This increases the "Fellowship Count" for that cider.
*   Allows for updating metadata and adding to the price history.

## Duplicate Prevention

*   The system will check for duplicates by name when a new cider is logged.
*   If a match is found, the user will be prompted to re-log the existing cider.
