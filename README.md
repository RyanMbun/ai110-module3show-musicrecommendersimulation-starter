🎵 Music Recommender Simulation

Project Summary

In this project I built and explained a small music recommender system.
My version is a content based recommender. It does not look at what other users liked, it just compares a song's own features (genre, mood, energy) to a user's stated taste profile and scores how close the match is. I built a 20 song catalog, a scoring function that hands out points for matches, and a ranking function that sorts songs from best to worst score. I tested it on five different user profiles, including one profile with contradicting preferences, to see if the scoring logic would break or just give weird but explainable results.


How The System Works

Each Song has these features:


genre (pop, rock, lofi, indie, edm, acoustic)
mood (happy, chill, sad, intense, energetic, melancholy, angry, nostalgic)
energy (a number from 0.0 to 1.0)
tempo_bpm, danceability, and acousticness (extra features I added for depth, not used in scoring yet)


Each UserProfile is just a dictionary with three target values:


favorite_genre
favorite_mood
target_energy (a number from 0.0 to 1.0)


To score a song, my Recommender does this:


If the song's genre matches the user's favorite genre, add 2.0 points.
If the song's mood matches the user's favorite mood, add 1.0 point.
Compare the song's energy to the user's target energy. The closer they are, the more points the song gets, up to 2.0 points if they match exactly.
Add all of that up into one score, and keep a list of reasons so I can see why a song scored the way it did.


To pick recommendations, I score every song in the catalog with the steps above, then sort the whole list from highest score to lowest score, and return the top k songs (I used k=5). Basically the flow goes:

User profile -> loop through every song and score it -> sort by score -> return top 5


Getting Started

Setup


Create a virtual environment (optional but recommended):


bash   python -m venv .venv
   source .venv/bin/activate      # Mac or Linux
   .venv\Scripts\activate         # Windows


Install dependencies


bash   pip install -r requirements.txt


Run the app:


bash   python -m src.main

Running Tests

Run the starter tests with:

bashpytest

You can add more tests in tests/test_recommender.py.


Sample Recommendation Output

Here is real output from running python -m src.main on my dataset:

Loaded songs: 20

=== Default (Pop/Happy) ===
User profile: {'favorite_genre': 'pop', 'favorite_mood': 'happy', 'target_energy': 0.7}
Recommendations:
  1. Sunlit Streets — The Paper Cranes  | score=4.80  (genre match (+2.0), mood match (+1.0), energy similarity (+1.80))
  2. Golden Hour — Sable June  | score=4.80  (genre match (+2.0), mood match (+1.0), energy similarity (+1.80))
  3. Firelight — Titan Pulse  | score=3.70  (genre match (+2.0), energy similarity (+1.70))
  4. Neon Nights — Voltage Kids  | score=3.60  (genre match (+2.0), energy similarity (+1.60))
  5. Gym Hero — Titan Pulse  | score=3.60  (genre match (+2.0), energy similarity (+1.60))

=== Chill Lofi ===
User profile: {'favorite_genre': 'lofi', 'favorite_mood': 'chill', 'target_energy': 0.2}
Recommendations:
  1. Quiet Rain — Marina Holt  | score=5.00  (genre match (+2.0), mood match (+1.0), energy similarity (+2.00))
  2. Study Haze — Kobo Beats  | score=4.90  (genre match (+2.0), mood match (+1.0), energy similarity (+1.90))
  3. Velvet Ash — Marina Holt  | score=3.80  (genre match (+2.0), energy similarity (+1.80))
  4. Sea Glass — Wren & Ash  | score=2.80  (mood match (+1.0), energy similarity (+1.80))
  5. Falling Slow — Wren & Ash  | score=2.00  (energy similarity (+2.00))

=== Conflicted (High Energy + Sad) ===
User profile: {'favorite_genre': 'acoustic', 'favorite_mood': 'sad', 'target_energy': 0.9}
Recommendations:
  1. Autumn Hollow — Wren & Ash  | score=3.70  (genre match (+2.0), mood match (+1.0), energy similarity (+0.70))
  2. Sea Glass — Wren & Ash  | score=2.80  (genre match (+2.0), energy similarity (+0.80))
  3. Falling Slow — Wren & Ash  | score=2.60  (genre match (+2.0), energy similarity (+0.60))
  4. Slow Burn — Ember Row  | score=2.20  (mood match (+1.0), energy similarity (+1.20))
  5. Neon Nights — Voltage Kids  | score=2.00  (energy similarity (+2.00))

(Full output for all 5 profiles, including High-Energy Pop and Deep Intense Rock, prints when you run the script.)

Screenshot or video (optional): <!-- Insert a screenshot or demo video link here -->


Experiments You Tried

Weight shift: I tried doubling the energy weight to 4.0 and cutting genre down to 1.0. The lists changed a lot for profiles like Chill Lofi, since songs like Falling Slow (low energy, no genre or mood match) jumped up the rankings just because their energy matched closely. This showed me the genre weight was doing most of the heavy lifting before, and energy alone is not a great substitute for it.

Conflicted profile test: I ran a profile that asked for acoustic genre, sad mood, but high target energy (0.9), which does not really exist in the catalog since my acoustic and sad songs are all low energy. The system did not break, it just gave its best partial matches and scored them lower overall (top score was 3.70 instead of 5.00). This is a good sign since it means the scoring rule degrades gracefully instead of crashing or returning nothing.

Small catalog effect: With only 20 songs, some artists (like Wren & Ash) show up multiple times in the same top 5 list. In a real system with millions of songs this probably would not happen as much, but it shows how a small dataset can make the recommendations feel repetitive.


Limitations and Risks


The catalog only has 20 songs, so recommendations repeat the same handful of artists a lot.
It has no idea about lyrics, vocals, or language, it only looks at genre, mood, and energy labels.
It is purely content based, so it can't pick up on things like "people who liked this also liked that."
Genre match is worth twice as much as mood match, so it probably over favors genre and can miss songs that have the right vibe but the wrong genre label.


I go deeper on this in my model card.


Reflection

Read and complete model_card.md:
Model Card

Building this made it click for me how a recommender is really just data plus a scoring rule plus a sort. There is no magic in it, it is just math applied to labels someone assigned to a song ahead of time. The genre and mood matches are just simple equality checks, and energy is the only part doing anything close to "smart" comparison.

The bias part is what stuck with me the most. Since my dataset has more pop songs than any other genre, and genre is worth the most points, pop songs naturally win more often even for users who did not ask for pop. That is not because pop is "better," it is just because of how I built the dataset and picked my weights. That is basically the same problem real platforms deal with at a much bigger scale, whatever gets more weight or more data ends up recommended more, and it can quietly shape what people listen to without anyone deciding that on purpose.
