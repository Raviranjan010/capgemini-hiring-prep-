# GLOBAL RULES (read fully before every task)

## Goal
Build a study repo for Capgemini's updated hiring assessment. Everything in _source/gemini_chat.md must appear in the repo, explained in easy words, with questions, options, answers and quick shortcuts. Nothing else.

## Source of truth
- _source/gemini_chat.md is the ONLY content source. Read only the parts needed for the current phase (search by heading). Do not re-read finished files.
- Never browse YouTube. Never invent video links, transcripts or facts about Capgemini.
- Language: simple English, short sentences, no jargon without a one-line meaning. (To get Hinglish explanations instead, change this line to: LANGUAGE = Hinglish.)

## Tags (put one at the top of every question or problem)
- [VIDEO] = Gemini says it came from a KN Academy video
- [CHAT] = discussed in the Gemini chat but not claimed from a video
- [ADDED] = standard question the chat only named or did not cover, written by you
- [UNVERIFIED] = a specific number or rule that Gemini stated without a source

## Question template (MCQ)
Title, tag, Question (with code if any), Options A to D, Correct Answer, Why (3 to 5 easy lines), 5-Second Shortcut (one line), Trap (one line).

## Debugging problem template
Title, tag, Video link (if verified), Problem statement, Sample input and output, Buggy code, Bugs found (numbered, each with why it is wrong), Fixed code (Java and C++ if the chat gave both, else the language given), Dry-run table, Spot-It-Fast rule (how to notice this bug in 10 seconds), Edge cases, Time and space.

## DO
- Keep every question, option, answer, buggy code, bug list, dry run, trick and cheat-sheet row that exists in the chat. Keep wording of questions and options as in the chat.
- Fix errors listed in "Known corrections" below.
- Every file must be complete: no "TODO", no "...", no "see chat", no half sections.
- Every code block must have a language tag and must be correct.
- Put an "Extra Practice" section at the END of a file only for [ADDED] questions, within the caps below.
- Use relative links between files.

## DON'T
- Do not add topics, tools, folders, badges, images, diagrams, CI, licenses, or long intros that are not asked.
- Do not exceed the caps for [ADDED] questions.
- Do not rewrite or "improve" the chat's explanations into longer text. Simplify, do not expand.
- Do not state [UNVERIFIED] items as official Capgemini rules.
- Do not call the repo "100% guaranteed". Add this line once in the main README: "Prep material, not the official syllabus. Real questions can differ."
- Do not create files outside the structure below.
- Do not ask questions back. If something is unclear, follow these rules and continue.

## Caps for [ADDED] questions (per file, maximum)
AI literacy 5 | networking+SQL+DBMS 4 | OOPs+OS 4 | pseudocode+bitwise 4 | debugging: only the items named below | coding patterns: only the items named below | cognitive extras 3 | communication extras 2 topics.

## Link policy
- Verified videos (the user gave these): https://youtu.be/o5TbT3kzEnA and https://www.youtube.com/watch?v=mLaYLknw4KU&list=PLGFjgYQtw1UjDBEkL2Edqej2kXbxGA1NO
- Four more links appeared only inside the Gemini script, so they are unverified. Show them in RESOURCES.md with status "UNVERIFIED - open and confirm": https://youtu.be/f_9-TT2hGQ4 (common debugging bugs), https://youtu.be/YEZy2e_PARE (height-balanced tree), https://youtu.be/SjbedEKjacM (2D matrix 7 bugs), https://youtu.be/Myw7Po8fyWw (harmonic subarray). In topic files, mention them only as "(unverified link, see RESOURCES.md)".
- External problem links allowed ONLY from this list (leetcode.com/problems/<slug>/): jump-game, jump-game-ii, gas-station, merge-intervals, balanced-binary-tree, binary-tree-level-order-traversal, binary-tree-zigzag-level-order-traversal, search-a-2d-matrix-ii, linked-list-cycle, subsets, n-queens, sliding-window-maximum, subarray-sum-equals-k, continuous-subarray-sum, subarray-sums-divisible-by-k, contiguous-array, single-number-iii, valid-parentheses, reverse-linked-list. No other external links.

## Known corrections (apply silently, add a one-line note "Corrected" where used)
1. Banker's Algorithm uses matrices (Available, Max, Allocation, Need = Max - Allocation) and checks for a safe sequence. It does NOT use a resource-allocation graph.
2. Merge Intervals Java fix: note that "current = intervals[0]" edits the input array; fine for the exam, but mention it in one line.
3. Digit Challenge and Grid puzzle examples were made up by Gemini, not shown in the video. Label them "Practice example (not from video)".
4. Communication-round numbers (2 or 3 second silence, "40% score drop", section names, 30s prep and 45s speaking) and the "15 to 18 correct MCQs" cutoff are [UNVERIFIED]. Show them as "commonly advised", never as rules.
5. Behavioral "always choose B" advice is a common prep heuristic, not an official key. Write it as "usually safer", and add one line: answer honestly and stay consistent, because the test checks consistency.
6. The chat says "Jump Game I buggy code from the exam". Treat the 3 bugs as [CHAT] unless the chat says video, and tell the reader to compare with the video.
7. "Some A are B" does NOT guarantee "Some A are not B". Keep this exact.

## Definition of done (per phase)
All listed files exist, every checklist item is covered, no placeholder text, code compiles (if java/g++ available), COVERAGE.md updated for that phase.

## Reply style after finishing a phase
Maximum 8 lines: files created, items covered, anything skipped and why.
