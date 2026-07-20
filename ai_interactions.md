AI Interactions Log


Stretch features only. Only fill in the sections that apply to stretch features you attempted. If you did not attempt a stretch feature, leave its section blank or delete it. This file is not required for the core project.




Agentic Workflow (SF8)


Document your experience using an AI agent (e.g., Cursor Agent, Claude, Copilot) to make multi-step changes autonomously.



What task did you give the agent?
I asked the agent to build out the whole recommender project in one go, not just one function at a time. That meant expanding my songs.csv into a bigger dataset, writing load_songs, score_song, and recommend_songs in recommender.py, building a main.py that runs several different user profiles through the recommender, adding starter tests, and then filling in my README and model_card.md with real output.

Prompts used:
The main prompt was basically: build the full project structure for this music recommender (expanded song dataset, recommender.py with load_songs/score_song/recommend_songs, a main.py that tests multiple profiles, tests, README, and model_card.md), then actually run it so I can see real output instead of a template. I followed up asking it to just give me the README file by itself once the full project was already built.

What did the agent generate or change?


data/songs.csv — expanded from a small starter file into 20 songs across pop, rock, lofi, indie, edm, and acoustic genres
src/recommender.py — load_songs, score_song, and recommend_songs functions, plus the weight constants for genre, mood, and energy
src/main.py — a CLI that loads the catalog and runs 5 different taste profiles through the recommender
tests/test_recommender.py — a few starter tests checking that matches score high, non matches score low, and results come back sorted
README.md and model_card.md — filled in with real terminal output instead of the placeholder text


What did I verify or fix manually?
I ran python -m src.main myself to check the output was real and not made up, and read through the scoring logic to make sure the genre/mood/energy weights actually matched what I wanted (genre worth 2 points, mood worth 1 point, energy scored on closeness). I couldn't run the pytest suite in the same environment since pytest wasn't installed and there was no network access to install it, so I still need to run pytest myself once I have it set up locally to confirm the tests actually pass. I also want to reread the README and model card wording and adjust anything that doesn't sound like something I'd actually say before I turn it in.


Design Pattern (SF10)

Not attempted for this project.
