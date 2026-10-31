# AI Fashion Stylist

A premium, **fully local** AI Personal Fashion Stylist. Upload a photo or use your webcam, and the app analyzes your skin tone, face shape and body proportions on-device — then recommends coordinated outfits with live shopping search links across Amazon India, Flipkart, Myntra and Meesho.

No accounts, no cloud uploads, no paid APIs. Everything runs in your browser using on-device face and body models (MediaPipe Tasks Vision).

## Features

- **Photo upload + live camera capture** (with mirrored preview).
- **Real skin analysis** — detects the face, samples cheek/forehead/chin skin patches, rejects non-skin pixels, and estimates depth (fair → deep) and undertone (warm / cool / neutral) from actual pixel data.
- **Face shape analysis** — uses 468 face landmarks to estimate oval, round, square, heart or long shapes with neckline guidance.
- **Body proportion analysis** — uses 33 pose landmarks to estimate shoulder/hip and torso/leg ratios and classifies the silhouette (triangle, inverted-triangle, rectangle, hourglass).
- **Height is never guessed from a photo** — you enter it yourself.
- **Personalized color palette** generated from your skin tone and undertone.
- **Recommendation engine** across shirts, pants, shoes and accessories, with per-item color and rationale tied to your profile.
- **Coordinated outfits** for Casual, Office/Formal and Smart Casual occasions.
- **Live shopping search links** — never fabricated prices or availability.
- **Premium black design** (`#080808` background, `#141414` cards, gold accent, Playfair Display + Inter typography), fully responsive.
- **Private** — your photo never leaves your device. The profile is saved in `localStorage`.

## How it works

The app uses Google's **MediaPipe Tasks Vision** library (`@mediapipe/tasks-vision`), which loads small on-device models (face_landmarker + pose_landmarker_lite) from a CDN on first run and runs inference on the CPU. On an Intel Core i5-13500H with 16 GB RAM and Iris Xe graphics, analysis completes in a few seconds with no GPU required.

- Skin pixels are sampled via Canvas `getImageData` around face landmarks, filtered by a robust RGB skin heuristic, then averaged to estimate tone and undertone.
- Face shape is derived from ratios of canonical face landmarks (forehead width, cheek width, jaw width, face length).
- Body proportions are derived from pose landmarks (shoulder, hip, knee, ankle) with visibility-based confidence.

When the photo can't support a reliable result (poor lighting, partial body, no face detected), the app says so plainly rather than inventing numbers.

## Installation (Windows)

You need [Node.js 18+](https://nodejs.org/) installed.

1. Open PowerShell or Command Prompt in the project folder.
2. Install dependencies:
   ```
   npm install
   ```
3. Start the app:
   ```
   npm run dev
   ```
4. Open the printed URL (usually `http://localhost:5173`) in Chrome or Edge.

That's it — one command to run.

**Note:** Camera access requires `https` or `localhost`. The Vite dev server runs on `localhost`, so the camera works automatically. If you deploy elsewhere, serve over HTTPS.

## Usage

1. Click **Start Styling**.
2. **Upload** a photo or **Open Camera** and capture one. For body analysis, use a full-body shot with arms slightly away from the torso and good lighting.
3. Enter your **height in cm** (we never estimate it from a photo).
4. Click **Analyze Me** — models load on first run (a few seconds).
5. Review your **Fashion Profile** (skin, face, body, palette).
6. **Pick clothing categories** (shirts, pants, shoes, accessories — multiple at once).
7. Click **Get Recommendations** for product cards with color, rationale and shopping links, plus coordinated outfits.
8. Click any **Shop Now** button to open the live search on Amazon, Flipkart, Myntra or Meesho.

Your profile is saved locally so you can return and pick new categories without re-analyzing. Click **Reset** in the header to clear everything.

## Tech stack

- **React 18 + TypeScript + Vite 7** — frontend.
- **Tailwind CSS 3** — styling.
- **MediaPipe Tasks Vision** — on-device face & body landmark detection (CPU).
- **lucide-react** — icons.
- **localStorage** — profile persistence (no database, no backend).

No Supabase, Firebase, OpenRouter or paid APIs are used.

## Scripts

| Command            | Description                          |
| ------------------ | ------------------------------------ |
| `npm run dev`      | Start the dev server                  |
| `npm run build`    | Production build into `dist/`        |
| `npm run preview`  | Preview the production build locally |
| `npm run typecheck`| TypeScript type-check only           |
| `npm run lint`     | Run ESLint                           |

## Limitations (stated honestly)

- Height is **not** measured from a photo — you enter it.
- Face/body classification is heuristic and works best with a clear, front-facing, well-lit photo against a plain background.
- Body proportion estimates are approximate when the legs are partially out of frame; the app flags this.
- Shopping links open a live platform search — we never claim specific prices, availability or product images.

## Privacy

Photos are processed entirely in your browser. No image data is uploaded to any server. Only the derived fashion profile (small JSON: skin tone, face shape, body ratios, palette) is stored in your browser's `localStorage`.
