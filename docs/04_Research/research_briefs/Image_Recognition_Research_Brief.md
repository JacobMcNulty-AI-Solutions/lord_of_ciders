# Research Brief: Image Recognition/OCR for Cider Logging

## Project Introduction: The Fellowship of the Cider - An Epic Journey of Cider Discovery

"The Fellowship of the Cider" is not just another logging app; it's a **gamified mobile application** designed to transform the simple act of trying ciders into an **epic medieval fantasy adventure**, deeply inspired by the rich lore of **Lord of the Rings**. Our core vision is to create a compelling, long-term engagement loop that encourages real-world cider exploration while providing a rich, immersive experience.

**Core Purpose**: To be the ultimate personal cider logging and documentation app, evolving into a comprehensive "cider dictionary" and gamifying the discovery process through unique mechanics.

**Key Gamified Mechanics**:
*   **Cider Logging & Rating**: Document every cider with detailed notes and a unique "tankard" rating system.
*   **Rarity System**: Discover and collect ciders across tiers from Common to Mythical/Artifact.
*   **Battle System**: Engage in "Pokemon-style" turn-based combat against "Orc beers" using your collected ciders.
*   **Quest & Achievement Systems**: Undertake weekly challenges and earn prestigious titles and rewards.
*   **Seven Rings of Cider**: Progress through a unique "ring" system tied to geographic exploration, style mastery, and battle victories.

This research is foundational to building the immersive and rewarding experience we envision.

## Research Area: Image Recognition/OCR for Cider Logging

### Research Goal
To investigate the feasibility, accuracy, and optimal implementation methods for using image recognition or Optical Character Recognition (OCR) to automatically detect cider names and brands from bottle labels during the logging process. This feature aims to significantly streamline the "Quick Log Mode" and enhance user convenience.

### Specific Questions to Answer

1.  **Feasibility & Expected Accuracy for Cider Labels:**
    *   What is the current state-of-the-art in mobile-based OCR or image recognition for reading text from complex, often curved, textured, or reflective surfaces like bottle labels?
    *   What level of accuracy can realistically be expected for detecting cider names, brands, and potentially other metadata (e.g., ABV, style, producer) from photos taken by users in varying real-world conditions (lighting, angle, label design, background clutter)?
    *   What are the inherent limitations of such technologies when applied to diverse cider labels (e.g., handwritten text, highly stylized fonts, glare, partial/obscured labels, foreign languages)?

2.  **Technology & API Selection (React Native Compatibility):**
    *   What third-party APIs or SDKs are available for mobile image recognition/OCR that are compatible with React Native (e.g., Google Cloud Vision API, AWS Rekognition, Microsoft Azure Computer Vision, Tesseract.js, specialized ML models)?
    *   What are the performance characteristics (speed of processing, latency) and cost implications (per-call pricing, data transfer costs) of these solutions?
    *   Can these solutions be integrated effectively to provide a smooth, near real-time user experience during the logging flow?

3.  **Implementation Workflow & User Experience:**
    *   What is the proposed user flow for integrating this feature into the "Quick Log Mode"? (e.g., user takes photo, app processes in background, suggests text overlay, user confirms/edits, then proceeds to rating).
    *   How will the recognized text be matched against the existing cider database (both hardcoded and user-added) for duplicate prevention and auto-filling metadata? What matching algorithms are suitable?
    *   What fallback mechanisms will be in place if recognition fails or is inaccurate, ensuring the user can still manually enter data without frustration?

4.  **Training Data & Custom Model Considerations:**
    *   If off-the-shelf solutions prove insufficient, would a custom ML model be necessary?
    *   If so, what kind of training data would be required (e.g., a large, diverse dataset of cider bottle labels with annotated text)? How feasible is it to acquire, generate, or label such data?

### Purpose of this Research for Design & Specification

This research is absolutely critical for enhancing the user experience and efficiency of "The Fellowship of the Cider" app. It will:
*   **Functional Specification Refinement:** Provide concrete details for significantly enhancing the "Quick Log Mode" (Section 4.2) and potentially other logging features, transforming a manual process into a semi-automated one.
*   **UI/UX Design (`03_Design/03_UI_UX_Design/`):** Directly inform the design of the camera interface, text suggestion overlays, and user confirmation steps, ensuring an intuitive and efficient logging flow.
*   **Technical Architecture (`functional_specification.md` Section 16 & `03_Design/01_Architecture_Design/`):** Influence crucial decisions on external API integrations, potential cloud functions for image processing, and overall data processing pipelines, ensuring scalability and responsiveness.
*   **User Experience & Adoption:** A successful implementation could dramatically reduce manual data entry, making the app faster, easier, and more enjoyable to use, thereby increasing user adoption and retention.
*   **Competitive Advantage:** This feature could differentiate "The Fellowship of the Cider" from other logging apps by offering a superior, more convenient logging experience.