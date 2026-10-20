CodeRank

Fair coding ratings that catch AI-assisted cheating.

Built by Team Eclipse.exe for Dexathon 2026 (Theme: Fintech & Edtech, Category: Software).

Live demo: https://kirupashankars.github.io/CODERANK/?utm_source=claude.com
<img width="1364" height="88" alt="image" src="https://github.com/user-attachments/assets/4a271fac-9e0c-450e-895e-a02c285ec794" />


The problem

Online coding tests are easy to cheat. A candidate can photograph a question, ask an AI on their phone, and type the answer back. A fast typist then scores as high as a genuinely skilled programmer, so scores stop meaning anything.

Our idea

CodeRank combines a chess-style Elo rating with a twist round that checks real understanding, like an oral test for code.

Original question. You type your own solution, which is tested automatically and timed.
Twist round. The same concept comes back as a new question, shown as your solution with key parts blanked out (fill in the blanks).
One blank at a time. Blanks unlock in order, and the options of a blank appear only when it is the active one. Typing speed gives no advantage, and a cheater has to go back to the phone for every blank.
Score and Elo. Your rating changes by correctness, time and steady answers. Suspicious patterns halve the score and are flagged for human review.
How scoring works

Elo rating (everyone starts at 1200, K = 32)

expected = 1 / (1 + 10^((problem rating - your rating) / 400))
new rating = old rating + 32 x (score - expected)

Twist round score

all blanks correct:  score = 0.4 + 0.4 x time + 0.2 x steady answers
otherwise:           score = 0.25 x (correct blanks / total blanks)
time = 1 - (time used + penalties) / limit        (limit = 45 s per blank)
Time credit is capped at 0.5 if the round is faster than 3 s per blank, because speed beyond human limits is not rewarded.
"Steady answers" drops when you change answers you already gave.
Every flag halves the score.
Anti-cheating checks
Check	Effect
Pasting into the editor (original round)	30 s cut from the timer
Leaving the tab or window	15 s cut from the timer
Too fast (under 3 s per blank)	Flag
Long, very regular pauses on every blank (looks like relaying each blank to another device)	Flag
Twist took more than 2.5 times as long as the original	Flag

A flag asks a human to review the result. It never bans anyone automatically.

Tech stack
HTML, CSS and vanilla JavaScript in a single file, with no framework and no build step
Browser focus and paste events for the activity checks

Production plan (not built yet): React front end with a code editor, Node.js and Express back end, PostgreSQL, sandboxed containers for running code, and an AI model (Claude API) to generate twist questions, solutions and wrong options.

How to run
Download coderank-blanks.html.
Open it in any modern browser. There is nothing to install.

To host it on GitHub Pages, rename the file to index.html, then go to Settings > Pages, choose the main branch, and save.

Project files
coderank-blanks.html   the prototype (page, question bank and scoring logic)
README.md              this file
Known limitations
The prototype has 3 problems with a pre-written question bank. Wrong options are checked by running the code. Generating twists live with an AI is the next step.
Everything runs in the browser, so timing and scoring could be tampered with. A real product must score on the server.
The original round accepts JavaScript answers only.
Your Elo rating resets when the page reloads.
The flag thresholds are starting guesses and need tuning with real-user data.
A cheater can still relay each blank to an AI slowly. The design raises the cost of cheating and flags suspicious cases, but it cannot stop a determined cheater.
Future work
AI-generated twists with automatic checking of every wrong option
Server-side scoring and sandboxed code execution
More problems and languages
Verified ratings through a short supervised test
Contests, leaderboards and a recruiter dashboard
Team

Team Eclipse.exe:
1. Kirupa Shankar S
2. Nihla N
3. Modis Jefrick Sounder Raja K
4. Kingmagimai Roshan G
5. Nanditha Srinivasan
6. Maryam Mohamed
