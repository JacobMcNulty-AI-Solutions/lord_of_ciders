# Image Recognition/OCR for Cider Logging

## Feasibility & Accuracy

Modern OCR can achieve very high accuracy on clear printed documents (often \>98% word accuracy [[medium.com](https://medium.com)]), but reading real-world cider labels is much harder. Cider labels are highly varied – with curved bottles, textured backgrounds and decorative fonts – which significantly reduces recognition reliability. For example, a study of wine label OCR noted that:

> “wine bottle labels can be particularly challenging for OCR, as they can vary drastically in style, font, language, and information displayed.” [[medium.com](https://medium.com)]

Experiments show curved or tilted text yields very low success rates (only \~20–42% of text correctly recognized on Android/iOS [[fritz.ai](https://fritz.ai)]). Similarly, mixed fonts on colored or shiny backgrounds often confuse OCR engines (some letters like “O” vs “G” may be misread [[medium.com](https://medium.com)]).

Nonetheless, advanced image-recognition hybrids have improved performance. For instance, the Vivino wine app combined OCR with image matching and boosted label scan accuracy from \~48% to \~86% [[prnewswire.com](https://www.prnewswire.com)]. On-device frameworks (Apple VisionKit on iOS or Google ML Kit) now support multi-language OCR and enhanced models that handle stylized text better [[vinetwine.ca](https://vinetwine.ca)].

In practice for cider labels, we should expect moderate accuracy: perhaps **70–90% on very clear cases**, but much lower (**20–50%**) on labels with severe warping or unusual design. Any automated text must be confirmed by the user; the app should highlight OCR results and let the user correct them.

## Technology & API Options (React Native)

A range of OCR tools can be used, each with trade-offs.

  * **On-Device OCR** (no network delay):

      * **Google ML Kit**: Has React Native bridges (e.g., [`@react-native-ml-kit/text-recognition`](https://www.google.com/search?q=%5Bhttps://www.npmjs.com/package/%40react-native-ml-kit/text-recognition%5D\(https://www.npmjs.com/package/%40react-native-ml-kit/text-recognition\))) and runs offline. In tests, ML Kit averaged \~140 ms per image on Android, faster than Tesseract (which took \~220 ms) [[designveloper.com](https://www.designveloper.com)].
      * **Apple Vision**: (iOS only) Highly optimized in hardware and uses updated ML models for OCR [[vinetwine.ca](https://vinetwine.ca)], but cannot be used on Android.
      * **Tesseract**: Open-source and can run on-device via RN wrappers (e.g., [`react-native-tesseract-ocr`](https://www.google.com/search?q=%5Bhttps://github.com/jonathanpalma/react-native-tesseract-ocr%5D\(https://github.com/jonathanpalma/react-native-tesseract-ocr\))), but it struggles with cursive/stylized fonts and is slower.

  * **Cloud OCR APIs** (Google Cloud Vision, AWS Rekognition/Textract, Azure Computer Vision):

      * These offload processing to servers, support many languages, and often have higher accuracy on complex text.
      * They incur network latency (often several seconds) and data-transfer costs.
      * **Pricing (per 1,000 images):**
          * Google Cloud Vision: \~$1.50 [[cloud.google.com](https://cloud.google.com/vision/pricing)]
          * AWS Rekognition: \~$1.00 [[aws.amazon.com](https://aws.amazon.com/rekognition/pricing/)]
          * Azure Computer Vision: \~$1.00 [[learn.microsoft.com](https://learn.microsoft.com/en-us/azure/ai-services/computer-vision/overview-ocr)]
      * These cloud calls add camera→server round-trips, so they should be done asynchronously.

### Recommendation

Use **on-device OCR** for speed and offline use. For Android, Google ML Kit is a strong default (good accuracy on standard fonts, \~140 ms processing [[designveloper.com](https://www.designveloper.com)]). If precision is critical or difficult fonts appear, fall back to a cloud API (e.g., Google Vision or AWS) in the background. React Native can integrate these via existing packages or REST calls.

## Performance & Cost

On-device OCR is nearly instant and cost-free per scan (aside from CPU usage). Cloud APIs have slight delays and add to app/network usage but are affordable at low volumes (\~0.1–0.2¢ per image [[cloud.google.com](https://cloud.google.com), [aws.amazon.com](https://aws.amazon.com)]). For a typical user with a few scans per session, the cost is negligible. The battery impact should be minimal for a quick text scan, but be mindful of not scanning every frame – trigger OCR on shutter only.

## Implementation Workflow & User Experience

A smooth flow might be:

1.  **User captures label photo.**
    In “Quick Log”, the user taps the camera and snaps the bottle label.

2.  **OCR runs automatically.**
    The image is processed (preferably on-device with ML Kit) immediately. Recognized text is overlaid on the photo or shown in a form field, allowing the user to review it.

3.  **User edits/confirms the text.**
    The app should present the detected cider name (and possibly ABV/style) in editable text inputs. Any low-confidence OCR segments should be clearly indicated. The user can correct mistakes or retake the photo. (Implement a confidence-based fallback: if OCR confidence is low, prompt the user to verify [[designveloper.com](https://www.designveloper.com)]).

4.  **Matching against the cider database.**
    After confirmation, the app compares the entered text to existing database entries using exact or fuzzy string matching (e.g., Levenshtein distance). Libraries like `Fuse.js` can rank likely matches. For example, if OCR reads “Somersbet Cider”, the fuzzy match might suggest “Somerset Cider”. Show a shortlist of the closest matches for the user to select.

5.  **Fallback for failed OCR.**
    If the photo yields garbage or no text, allow the user to switch to manual entry seamlessly. If OCR detects nothing, display empty input fields with a hint: “Tap to enter cider name manually.” Always let the user confirm or correct the final name before saving.

> **Best Practice:** Automatically capturing and filling data (as Vivino does [[prnewswire.com](https://www.prnewswire.com)]) greatly speeds logging, but it must be paired with easy user edits. Fallback prompts on low confidence improve reliability [[designveloper.com](https://www.designveloper.com)].

## Training Data & Custom Models

Off-the-shelf OCR/vision solutions should be tried first. A custom ML model (fine-tuned to cider labels) would require a large, annotated dataset of cider bottle images. No public cider-label dataset exists, and building one would be costly and time-consuming. For context, a recent “Alcoholic Beverage” classifier was trained on \~191k images [[pmc.ncbi.nlm.nih.gov](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC8909477/)], illustrating the scale needed for robust vision models.

### Recommendation

Initially rely on generic OCR. If scanning performance is unsatisfactory after tuning (e.g., cropping, lighting guidance), consider collecting user-submitted label photos over time. These can later be used to train a specialized model. For a first pass, focus on **robust UI fallbacks rather than custom training.**

## Summary & Recommendations

In summary, image-based OCR on cider labels is feasible but imperfect. To maximize success:

  * **Use on-device OCR first** (Google ML Kit on Android), which is fast and free. Supplement with cloud OCR for difficult cases.
  * Integrate OCR into the Quick-Log flow by showing **editable text overlays** and auto-suggestions from your cider DB using fuzzy matching.
  * **Always allow user verification and correction.** Provide clear cues when confidence is low [[designveloper.com](https://www.designveloper.com)].
  * **Prepare fallback flows:** manual text input, re-scan prompts, etc.
  * Monitor recognition results and iterate on pre-processing (cropping, auto-straightening, glare reduction) to improve OCR input quality.