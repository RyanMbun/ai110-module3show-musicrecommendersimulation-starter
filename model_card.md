🎧 Model Card: Music Recommender Simulation

1. Model Name

VibeFinder 1.0


2. Intended Use

This recommender is designed to take a simple taste profile (favorite genre, favorite mood, target energy) and return the top 5 songs from a small catalog that match it best. It generates content based recommendations, meaning it only compares a song's own labeled features to the user's stated preferences, it does not look at what other users listened to or liked. It assumes the user can actually name their favorite genre, mood, and a rough energy level, which is a pretty simplified way to describe taste. This is built for classroom exploration, not real users, since the catalog is tiny and the scoring rules are simple on purpose so I could actually explain and test them.


3. How the Model Works

Every song has a genre, a mood, and an energy level from 0 to 1. The user profile has a favorite genre, a favorite mood, and a target energy. To score a song, the model checks if the genre matches (worth 2 points), checks if the mood matches (worth 1 point), and then looks at how close the song's energy is to the target energy (worth up to 2 points, the closer the match the more points). All those points get added up into one score, and every song in the catalog gets scored the same way. Then the songs just get sorted from highest score to lowest score and the top 5 get returned. From the starter logic, the main thing I changed was making genre worth more than mood, and making energy a "closeness" score instead of a simple yes or no match, since energy felt more like a sliding scale than a category.


4. Data

The catalog has 20 songs. Genres include pop, rock, lofi, indie, edm, and acoustic, and moods include happy, chill, sad, intense, energetic, melancholy, angry, and nostalgic. I added 10 songs on top of the original starter set to cover more genres and moods that were not represented before. Even with the expansion, pop and rock still have more songs than the other genres, so the catalog is not evenly balanced. Things like lyrics, vocal style, language, and cultural context are completely missing from the dataset, the model only ever sees genre, mood, and energy labels.


5. Strengths

The system gives reasonable results for users who have a clear, single genre and mood preference, like someone who wants chill lofi or intense rock. It seems to correctly capture the idea that energy should be a sliding scale rather than a strict match, since songs with close but not identical energy still show up in the top results instead of getting completely ignored. When I tested profiles like Chill Lofi and Deep Intense Rock, the top results matched my own intuition pretty well, the chill lofi tracks came out on top for the lofi profile and the highest energy rock tracks came out on top for the intense rock profile.


6. Limitations and Bias

The model does not consider lyrics, vocals, tempo, danceability, or acousticness in its scoring, even though some of those are in the dataset. Genres like edm and acoustic are underrepresented compared to pop and rock, so users with those preferences have a smaller pool to pull good matches from. Since genre is worth the most points, the system can overfit to genre and miss songs that have the right mood or energy but the wrong genre label. Because pop has more songs in the catalog, it tends to show up in top results more often even for profiles that were not really asking for pop, which is a bias that comes from the data, not something anyone decided on purpose.


7. Evaluation

I tested 5 profiles: Default (Pop/Happy), High-Energy Pop, Chill Lofi, Deep Intense Rock, and a Conflicted profile that asked for acoustic genre, sad mood, but high energy, which does not really exist together in the catalog. I looked at whether the top 5 results made sense for each profile and whether close profiles (like Default vs High-Energy Pop) produced different but reasonable rankings. What surprised me was the Conflicted profile did not break or return nothing, it just gave lower scores overall and picked the closest partial matches it could, which showed me the scoring degrades gracefully instead of failing outright. I also ran one small experiment where I doubled the energy weight and cut genre in half just to see how sensitive the rankings are, and the results shifted quite a bit, which told me the current weights are doing a lot of the actual decision making.


8. Future Work

I would add more songs, especially in the underrepresented genres like edm and acoustic, so the catalog is more balanced. I would bring tempo, danceability, and acousticness into the actual scoring instead of just storing them unused. I'd also want a way to boost diversity in the top 5, right now the same handful of artists can dominate a list since there is nothing stopping similar songs from all showing up together. For more complex tastes, I'd want to let users list more than one favorite genre or mood instead of forcing a single value for each.


9. Personal Reflection

Building this showed me that a recommender is really just labeled data plus a scoring rule plus a sort, there is no hidden magic to it. The genre and mood checks are just simple equality checks, and energy is the only part doing anything close to real comparison. What surprised me the most was how much the weights alone shape the output, changing genre from 2.0 to 1.0 changed the top results a lot even though nothing about the songs themselves changed. It made me think differently about apps like Spotify or TikTok, since the recommendations we see are not some neutral reflection of "what's good," they are a direct result of what got weighted more and what data was available in the first place.
