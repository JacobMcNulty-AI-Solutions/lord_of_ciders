# Summary: Offline Capability and Data Sync Strategy

## Key Findings
This document details strategies for implementing robust offline capabilities and seamless data synchronization for the app. It covers local data storage options, synchronization mechanisms, conflict resolution, user experience in offline mode, and performance optimization.

-   **Offline Data Storage**: 
    -   **AsyncStorage**: Simple key-value, suitable for small data, but not a full database.
    -   **SQLite**: Proven relational DB, powerful but complex native setup.
    -   **WatermelonDB**: High-performance, reactive, built for offline-first with lazy loading and sync engine.
    -   **Realm**: Object-oriented, fast reads/writes, offline-first, but can increase app size.
    -   **Images/Files**: Store on device file system, not directly in DB; store paths in DB.
-   **Synchronization Mechanisms**: 
    -   **Push/Pull Sync**: Efficiently merges offline changes with backend. Tracks `updated_at` timestamps or "dirty" flags.
    -   **Delta-changes**: Only transfer changes, not entire dataset.
    -   **File Uploads**: Use robust protocols like TUS for resumable uploads of large files.
-   **Conflict Resolution**: 
    -   **Last-Write-Wins**: Simple and effective for single-user apps, using `updated_at` timestamps.
    -   **Custom Logic**: For specific data types (e.g., numeric counters).
    -   **Soft-deletes**: Use `deleted_at` timestamps for proper reconciliation of removals.
-   **User Experience in Offline Mode**: 
    -   **Minimum Functionality**: Logging new ciders, browsing existing local data.
    -   **UI Indicators**: Clear, non-intrusive communication of offline status, pending syncs, and potential conflicts.
-   **Performance & Battery Life Optimization**: 
    -   **Scheduled Syncs**: Only sync when necessary (e.g., after logging, app resumes).
    -   **Batching**: Group multiple data operations into single network calls.
    -   **Image Compression**: Resize/compress photos before upload.
    -   **Background Processing**: Offload heavy tasks to background threads.

## Actionable Insights for System Design

1.  **Choose WatermelonDB for Local Storage**: Given its high performance, reactive nature, and built-in sync engine, WatermelonDB is the recommended choice for the local database in React Native, especially for an offline-first application with complex data models.
2.  **Implement Push/Pull Sync with Delta-Changes**: Design the synchronization logic around a push/pull model, tracking changes using `updated_at` timestamps or "dirty" flags to ensure efficient data transfer and reliable merging with the Supabase backend.
3.  **Separate Image Storage**: Store images on the device's file system and only their paths/URIs in WatermelonDB. Implement TUS for resumable, robust image uploads to the backend storage.
4.  **Adopt Last-Write-Wins for Conflict Resolution**: For most data, use a Last-Write-Wins strategy based on `updated_at` timestamps. For specific cases like counters, implement custom merging logic. Use soft-deletes for record removal.
5.  **Prioritize Core Offline Functionality**: Ensure users can log new ciders and browse their existing collection seamlessly when offline. This is the minimum viable functionality.
6.  **Design Clear Offline UI/UX**: Implement non-intrusive UI elements (e.g., a small banner, sync icon with count) to clearly communicate offline status, pending synchronizations, and any sync-related messages to the user.
7.  **Optimize Sync for Performance and Battery**: 
    -   Schedule syncs intelligently (e.g., on app resume, after logging, or using background fetch APIs).
    -   Batch multiple data changes into single network requests.
    -   Automatically compress images before upload.
    -   Utilize background processing for heavy sync tasks to keep the UI responsive.
8.  **Implement Database Migrations**: Plan for schema changes with migrations to prevent data loss during app updates and ensure data integrity.
9.  **Indexing for Performance**: Create indexes on frequently queried fields in the local database to accelerate searches and improve overall app responsiveness.