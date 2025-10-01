# Dreamio Product Requirement Document (PRD)

## Project: Bedtime Story Web App (PWA)

**Target Users:** Parents and children 

**Platform:** Web Application (PWA), 100% mobile-friendly (parent's phone or child’s iPad)

**Tech Stack:** Next.js, TypeScript, Supabase

**Automation:** n8n workflows (story image generation with ChatGPT-based pipeline & voice generation with ElevenLabs)

**Hosting & Deployment:** Vercel (production + preview deployments)

**CI/CD:** Vercel’s off‑the‑shelf CI/CD via GitHub integration (auto builds on pushes/PRs with preview URLs). **Husky** pre‑commit hook enforces linting & type checks: `eslint .`, `prettier --check .`, and `tsc --noEmit` before commits.---

## 1. Overview

This product is a Progressive Web App (PWA) designed to help parents create and play personalized bedtime stories for their kids. The app combines storytelling, visuals, and soothing audio to create an immersive, calm, and engaging bedtime experience.

---

## 2. Core Features

### 2.1 Story Playlist

* Parents can maintain a playlist of previously created stories.
* Stories can be sorted by creation date, favorites, or child preferences.
* Stories can be replayed anytime from the generated MP3 files.

### 2.2 Story Playback

* Stories are presented with **voice narration** and **storyline-relevant images**.
* Images change with a **slow fade mode** (minimal animations to avoid distraction).
* Subtitles (story text) can be toggled **on/off**.
* Play mode is similar to **PowerPoint/Google Slides**.

  * Parent can manually advance pages (initial version).
  * Auto-slides sync with audio once narration is generated.

### 2.3 Story Creation (ChatGPT-like Interface)

* Parent guides child to provide prompts via **text or voice clips**.
* A ChatGPT-like interface builds the story iteratively.
* Multiple prompts enhance and expand the story.
* Once completed, the **story text is generated**.

### 2.4 Story Image Generation

* Button: **"Create Images"**
* Story text is divided into storyboards.
* Each storyboard generates one image:

  * If no child image provided → default character.
  * If child photo provided → consistent character illustrations taken from a prompt
* Images stored in Supabase storage and URL stored in Supabase database.

### 2.5 Voice Generation

* Button: **"Generate Voices"**
* Uses ElevenLabs TTS based on a dropdown of voices, and prompts, and instructions.
* Predefined **slow, soothing bedtime voice**.
* Audio files saved to Supabase storage and the URL stored in Supabase database.

### 2.6 Music & Soundtrack Playback

* Some stories may include background soundtracks. Maintain a predefined playlist.
* Story playback synchronizes images, voice, and soundtrack.
* Parents/children can toggle background music.

### 2.7 Subtitles

* Toggle option during playback.
* Supports accessibility and reading practice.

---

## 3. Workflows (n8n)

### Workflow 1: Image Generation

1. Pull story text from Supabase.
2. Use AI agent node to chunk story into scenes.
3. Generate consistent character images using **Nano Banana (or ChatGPT-based pipeline), TBD**.
4. Follow style reference: [Consistent AI Image Characters](https://youtu.be/K4cW77-_Q7I), or find a better instruction.
5. Save generated images to Supabase storage.
6. Store image URLs in Supabase database.

### Workflow 2: Voice Generation

1. Pull story script from Supabase.
2. Generate voice narration using **ElevenLabs** (slow bedtime TTS voice).
3. Save audio files in Supabase storage.
4. Store audio file URLs in Supabase database.

---

## 4. User Flows

### Story Creation Flow

1. Parent selects **"Create New Story"**.
2. Child provides imagination prompts (text/voice).
3. Parent submits prompts in ChatGPT-like interface.
4. System generates story text.
5. Parent iteratively adds more prompts (optional).
6. Once final → Parent clicks **"Create Images"**.
7. Storyboard images are generated.
8. Parent optionally uploads children’s photos for personalized characters.
9. AI creates consistent characters using this method: [https://youtu.be/K4cW77-_Q7I](https://youtu.be/K4cW77-_Q7I)

### Story Playback Flow

1. Parent selects story from playlist.
2. Story opens in slideshow mode.
3. Parent reads story manually (initial version).
4. Optionally: Parent clicks **"Generate Voices"**.
5. System generates narration → playback syncs automatically with slides.
6. Parent gives instructions to generate voices using ElevenLabs TTS scripting
7. Subtitles on/off toggle available.
8. Add a background soundtrack from a list

---

## 5. Non-Functional Requirements

* **Performance:** Load time under 2s for story playback.
* **Scalability:** Support multiple stories and media assets.
* **Consistency:** Images must maintain consistent characters across storyboards.
* **Reliability:** Supabase storage ensures persistence of story data.
* **Accessibility:** Subtitles, voice narration, and mobile responsiveness.
* **Security:** Safe storage of children’s images and audio (Supabase access control).

---

## 6. Future Enhancements

* Personal voice cloning for parents.
* Background lullaby generator.
* Story sharing with family/friends.
* Offline mode support (PWA advanced caching)
