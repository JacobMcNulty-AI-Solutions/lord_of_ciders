# Summary: Image Recognition/OCR for Cider Logging

## Key Findings
This document explores the feasibility, accuracy, technology options, performance, cost, and user experience considerations for implementing image recognition (OCR) for cider label logging in the app.

-   **Feasibility & Accuracy**: OCR on cider labels is challenging due to varied designs (curved bottles, textured backgrounds, decorative fonts). Accuracy is expected to be moderate (70-90% for clear cases, 20-50% for difficult ones). User confirmation of OCR results is essential.
-   **Technology Options**: 
    -   **On-Device OCR**: Google ML Kit (Android) and Apple Vision (iOS) are recommended for speed and offline use. Tesseract is open-source but slower and struggles with stylized fonts. 
    -   **Cloud OCR APIs**: Google Cloud Vision, AWS Rekognition/Textract, Azure Computer Vision offer higher accuracy for complex text but incur network latency and costs. They can serve as a fallback.
-   **Performance & Cost**: On-device OCR is nearly instant and cost-free. Cloud APIs have slight delays and minimal cost per scan at low volumes. Battery impact should be minimal if OCR is triggered only on shutter.
-   **Implementation Workflow & UX**: A smooth workflow involves user capture, automatic on-device OCR, user editing/confirmation of text, matching against a cider database (fuzzy matching), and a seamless fallback to manual entry if OCR fails.
-   **Training Data & Custom Models**: Custom ML models for cider labels are not recommended initially due to the lack of public datasets and high cost. Rely on generic OCR with robust UI fallbacks.

## Actionable Insights for System Design

1.  **Prioritize On-Device OCR**: Implement Google ML Kit for Android and Apple Vision for iOS as the primary OCR solution for speed and offline capability. This ensures a responsive user experience.
2.  **Cloud OCR as Fallback**: Design the system to optionally use a cloud OCR API (e.g., Google Cloud Vision) as a fallback for cases where on-device OCR yields low confidence or fails completely. This can be done asynchronously.
3.  **User-Centric Correction UI**: Develop a clear and intuitive user interface for reviewing and correcting OCR results. Highlight low-confidence segments and provide editable text fields for the detected cider name, ABV, and style. This is critical for maintaining data accuracy.
4.  **Seamless Manual Entry Fallback**: Ensure a smooth transition to manual data entry if OCR is unsuccessful or the user prefers it. Provide clear prompts like "Tap to enter cider name manually."
5.  **Fuzzy Matching for Database Lookup**: After OCR and user confirmation, use fuzzy string matching (e.g., Levenshtein distance with libraries like `Fuse.js`) to compare the recognized text against the app's cider database and suggest likely matches.
6.  **Image Pre-processing**: Implement basic image pre-processing steps (e.g., cropping, auto-straightening, glare reduction) to improve the quality of input for the OCR engine.
7.  **Optimize Performance**: Trigger OCR only when the user explicitly captures a photo, not continuously. Compress or resize images before uploading to minimize network usage and battery drain.
8.  **Iterative Improvement**: Plan to monitor OCR performance and user feedback. If generic OCR proves insufficient over time, consider collecting user-submitted label photos to potentially train a specialized model in the future.
9.  **Offline Logging Integration**: The on-device OCR capability is crucial for enabling the "Quick Log" feature to function effectively even when the user is offline, aligning with the offline sync strategy.