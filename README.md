**LabLink** is a laboratory equipment discovery, booking, approval, and conflict-resolution platform designed to solve five core problems: equipment discovery, intelligent slot booking, project-based access requests, faculty approvals that optimize utilization, and fair conflict resolution based on deadlines, availability, and requirements. Built on **Next.js App Router** (TypeScript strict mode), **Firebase Auth**, **Firestore**, **Zod**, **Vitest**, and the **Gemini SDK**—deployed as a single service on **Cloud Run**—it follows a strict architectural split where **AI only suggests and explains, while deterministic engine code and database transactions enforce rules and state changes**.

 Key Pillars & Features

* **Transaction-Safe Booking Engine:** Supports multi-item bookings (all-or-nothing confirmation), half-open interval overlap checks with buffers, a single transition map for booking states, automatic expiry/no-show housekeeping, and atomic Firestore transactions to prevent double bookings.


* **Grounded AI Lab Matcher:** Takes a sanitized project description and a compact catalog, makes a single structured Gemini call validated with Zod, filters out invalid/unavailable/incompatible items in code, checks slot availability, and hands picks to the booking form (with a deterministic fallback if Gemini fails).


* **Deterministic + AI Conflict Resolver:** Triggered by overlapping pending requests or maintenance hitting confirmed bookings; ranks feasible allocations by hard deadline feasibility, fewest students moved, smallest time shift, earlier deadline, fewer recent equipment hours, and earlier submission, while using Gemini only to explain the rule trace.


* **Role-Based Access & Operations:** Enforces server-side permissions via a central `can(user, action, resource)` check across `unassigned`, `student`, `faculty` (including `labManager` duties for issuing/returning gear), and `admin` roles, alongside faculty approval queues, maintenance windows, append-only audit logs, in-app notifications, and utilization analytics computed from actual issue/return hours.


* **Quality, Security & Accessibility:** Designed around six judging criteria—code quality, security (deny-all client Firestore rules, Secret Manager, strict Zod validation, rate limits, `DEMO_MODE` custom tokens), efficiency (~10 runtime dependencies, pagination, caching, multi-stage Docker build), Vitest testing, WCAG 2.2 AA accessibility, and problem alignment—built via an eight-prompt Antigravity workflow and CI pipeline.
