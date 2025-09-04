# Technical Stack

This document outlines the technical stack for The Fellowship of the Cider application.

## Platform & Framework

*   **React Native + Expo**: For cross-platform mobile development.
*   **Android-focused**: The primary target platform for initial development and testing.

## Backend & Database

*   **Supabase**: For backend-as-a-service (BaaS), providing database, authentication, and storage.
*   **Direct Frontend-to-Supabase Communication**: No separate backend API will be built. The React Native application will communicate directly with the Supabase API.
