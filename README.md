# HiringAI

https://docs.google.com/document/d/1QaNP4krKCvgHJ8a-3XxNLYXKTSJuByF7JyETxWDC88s/edit?usp=drivesdk

1. SAMPLE_CANDIDATES
Hardcoded data of 3 candidates — used to populate the dashboard so it's not empty on load.
2. STATUS_CONFIG
Colour scheme for each status — green for Shortlisted, red for Rejected, yellow for Manual Review.
3. ScoreBadge
The circular score display (like 91, 62, 95) — colour changes based on score range.
4. StatusPill
The small coloured label that shows SHORTLISTED / REJECTED / MANUAL REVIEW.
5. UploadForm
The candidate application page — form fields + PDF drag and drop + submit button.
6. ParsingView
The AI screening screen — 5 step animation → generates result → shows score, skills, status, and auto-drafted email.
7. Dashboard
Full recruiter panel — stats cards, search bar, filter buttons, candidate table, and clickable side panel with status changer and notes.
8. App (root component)
The main controller — manages which screen is visible (upload / parsing / dashboard) and passes data between all components.
