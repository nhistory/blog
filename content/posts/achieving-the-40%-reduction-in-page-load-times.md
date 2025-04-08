+++
date = '2024-04-07T23:01:07-03:00'
title = 'Achieving the 40% Reduction in Page Load Times'
+++

# Initial Problem: The Yeti dashboard, displaying potentially large datasets (like the >100k transaction records mentioned), suffered from slow initial load times and sluggish interactions. This was due to a combination of large JavaScript bundles, inefficient data fetching, and unoptimized rendering.

Specific Actions Taken (Full-Stack Perspective):

Leveraging Next.js App Router & Server Components:

Before: Significant portions of the dashboard might have been rendered client-side, requiring large JS bundles and client-side data fetching (useEffect hooks fetching large amounts of data).

Action: Refactored key dashboard sections using Next.js Server Components. Heavy data fetching logic for the initial view was moved to the server. This drastically reduced the amount of JavaScript shipped to the client for the initial render, as rendering logic and data fetching happened server-side. Used React Suspense with Server Components for better loading state handling during server-side data fetching.

Impact: Reduced Time to Interactive (TTI) and First Contentful Paint (FCP).

Optimizing Data Fetching & Payload Size:

Before: The API might have returned the entire large dataset (or a very large chunk) for the initial dashboard view, even if only a fraction was immediately visible.

Action: Collaborated on API refinement (or implemented backend changes yourself, being full-stack). Introduced pagination and server-side filtering/sorting for the primary data tables/visualizations. The frontend now only requested the specific data needed for the current view (e.g., first 50 rows, data for the selected date range). Implemented backend caching for frequently requested, static-like data.

Impact: Smaller network payloads, faster API response times perceived by the frontend.

Code Splitting & Lazy Loading Client Components:

Before: Large charting libraries (e.g., Chart.js, D3) or complex UI components (like advanced data grids from MUI or other libraries) were bundled into the main client-side JavaScript, loading even if not immediately needed.

Action: Utilized next/dynamic to dynamically import heavy client-side components (e.g., complex charts, modals, detailed view panels). These components were only loaded when the user interacted with the feature requiring them (e.g., clicking a button to open a detailed chart view).

Impact: Reduced initial JavaScript bundle size, deferring the loading of non-critical code.

Asset Optimization:

Before: Large images or icons might not have been optimized. Custom CSS could have been bulky.

Action: Ensured all images were served using next/image for automatic optimization (resizing, modern formats like WebP). Audited and optimized CSS, ensuring efficient use of MUI's styling solution (e.g., leveraging theme variables, minimizing overrides) and ensuring proper tree-shaking for MUI components to only include necessary styles/JS.

Impact: Faster asset downloads, quicker rendering.

Caching Strategies:

Action: Fine-tuned Next.js caching. Leveraged Route Segment Config options in the App Router to apply appropriate caching (static rendering where possible, ISR for data that updates periodically, or dynamic rendering with server-side caching headers for personalized/highly dynamic data). Configured browser caching for static assets effectively.

Impact: Faster subsequent page loads and navigations within the app.

Combined Result: These combined efforts significantly reduced the amount of data transferred, the amount of JavaScript parsed and executed on the client, and the time spent waiting for server responses, leading to the measured 40% reduction in page load times.
