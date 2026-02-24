# BharatPath
🚂 BharatPath: Offline-First Railway Utility App

BharatPath is a mission-critical, offline-first mobile application designed to solve the problem of poor internet connectivity on Indian Railways. It shifts heavy computational tasks (like routing and proximity alerts) to the user's device, ensuring reliability in dead zones, while securely syncing social features via the cloud.

🎯 The Problem & Vision

Existing railway applications rely entirely on real-time cloud APIs. When a train enters a low-network zone, passengers lose access to crucial information, routing alternatives, and emergency services.

The Vision: Build a "Hybrid-Edge" architecture where the application's core brain—a master station graph and routing engine—lives locally on the device, requiring zero internet for critical operations.

✨ Core Features & Technical Implementation

1. P2P Seat Exchange (Zero-Knowledge Matchmaker)

What it does: Allows confirmed passengers on the same train to securely swap berths (e.g., Upper to Lower).

Tech Logic: Uses a SHA-256 client-side hashing engine. Raw PNRs never leave the phone. Supabase acts as a real-time WebSocket broker matching identical hashes.

2. Recursive Quota Hack (Smart Booking)

What it does: Finds "hidden" confirmed seats when direct routes are waitlisted.

Tech Logic: Backtracks through the Master Station Map to identify previous major junctions, launching recursive queries to find split-booking opportunities ("Book from X, Board at Y").

3. Smart Proximity POI (Offline Geofencing)

What it does: Alerts users to nearby hospitals or hotels as they approach their destination.

Tech Logic: A background service uses the Haversine formula to calculate spherical distance against local GPS. Triggers an Overpass API fetch only when < 10km from the destination.

4. Connecting Journey Finder (Local BFS Engine)

What it does: Calculates multi-train connecting routes entirely offline.

Tech Logic: Utilizes a local MMKV-stored Adjacency List of 4500+ stations. A custom Breadth-First Search (BFS) algorithm finds paths, enforcing a strict 120-minute safety buffer between connections.

5. Context-Aware Complaints (One-Tap Help)

What it does: Auto-fills 90% of a grievance report (RailMadad).

Tech Logic: Extracts Coach and Seat from the local user context and appends precise Lat/Lon from the GPS sensor before pushing to the cloud.

🏗️ System Architecture

BharatPath uses a Hybrid Data Architecture:

Edge Storage (MMKV): Houses the flat master_stations.json graph, POI ephemeral cache, and transient user context. Ensures <200ms latency for BFS routing.

Cloud Storage (Supabase/PostgreSQL): Handles the relational data required for multi-user interactions (p2p_seat_swaps and smart_complaints_log).

📂 Recommended Project Structure

BharatPath/
├── src/
│   ├── engines/           # Core offline logic (BFS, Haversine, SHA-256)
│   │   ├── bfsRouter.ts
│   │   ├── geoFencing.ts
│   │   └── privacyVault.ts
│   ├── services/          # API & Cloud bridges
│   │   ├── supabase.ts
│   │   └── overpassApi.ts
│   ├── components/        # Reusable UI (Tailwind/NativeWind)
│   ├── screens/           # Main application views
│   └── store/             # MMKV Local State Management
├── assets/                # Local JSON maps & images
└── docs/                  # Architecture diagrams (HLD/LLD)


🚀 Getting Started

Prerequisites

Node.js (v18+)

React Native CLI / Expo environment setup

Supabase Account (for database syncing)

Installation

Clone the repository:

git clone [https://github.com/YourUsername/BharatPath.git](https://github.com/YourUsername/BharatPath.git)
cd BharatPath


Install dependencies:

npm install


Set up environment variables (Create a .env file):

SUPABASE_URL=your_supabase_url
SUPABASE_ANON_KEY=your_anon_key


Run the application:

npx react-native run-android # or run-ios


Built as a Major Project for defining the future of offline-first utility engineering.
