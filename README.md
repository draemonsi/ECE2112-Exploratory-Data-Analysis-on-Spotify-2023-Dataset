# Exploratory Data Analysis on Spotify 2023 Dataset
### Intended Learning Outcomes
- Gain familiarity with exploratory data analysis (EDA) techniques to assess a music platform dataset.
- Analyze and visualize popular Spotify tracks, drawing insights on what makes a track popular.
- Understand correlations between musical attributes and popularity metrics like streams.

---
### Problem Description
This project involves conducting an Exploratory Data Analysis (EDA) on a dataset containing information on Spotify's most-streamed songs of 2023. The objective is to uncover patterns and trends, analyzing the factors contributing to track popularity. The dataset is sourced from [Kaggle: Most Streamed Spotify Songs 2023.](https://www.kaggle.com/datasets/nelgiriyewithana/top-spotify-songs-2023)

---
### Files Included
- **spotify_eda.ipynb:** Jupyter Notebook containing the EDA.
- **spotify-2023.csv:** Dataset with popular Spotify tracks.
- **README.md:** Documentation of the project.

---
### Technologies Used
- **Python Libraries:** Pandas, Matplotlib, Seaborn
- Jupyter Notebook

---
### Documentation
_**1. Importing Libraries**_
```# Import necessary libraries for data analysis and visualization
import pandas as pd       # For data manipulation and analysis
import numpy as np        # For numerical operations and array handling
import matplotlib.pyplot as plt  # For generating visualizations
import seaborn as sns     # For statistical data visualization
```
In the initial step, essential libraries for data analysis and visualization are imported. pandas is used for data manipulation, allowing for structured operations on data tables, while numpy supports numerical operations and array handling, making mathematical calculations more efficient. Visualization libraries matplotlib.pyplot and seaborn are imported to create plots and graphs; matplotlib is fundamental for general plotting, while seaborn adds a layer of statistical visualization that simplifies the creation of aesthetically pleasing and informative plots.

_**2. Loading and Previewing the Dataset**_
```
# Load the Spotify 2023 dataset
spotify_data = pd.read_csv('spotify-2023.csv')

# Display basic information about the dataset
# This step helps identify the structure, column names, and data types
print(spotify_data.info())
print(spotify_data.head())
```
After importing libraries, the Spotify dataset is loaded using pandas.read_csv(). This function reads the CSV file and stores it as a DataFrame called spotify_data, which allows for tabular data manipulation. The info() function provides an overview of the dataset, including the number of entries, column names, data types, and non-null values. Meanwhile, head() displays the first few rows to familiarize us with the data’s structure and typical values, which is essential in understanding the dataset's layout and identifying any initial anomalies or patterns.

_**3. Data Cleaning and Preprocessing**_
```
# Check for missing values in the dataset
# This step helps in identifying columns that may need imputation or removal
missing_values = spotify_data.isnull().sum()

# Fill or drop missing values as required
# Example: Removing rows with missing 'key' values or filling with mode
spotify_data['key'].fillna(spotify_data['key'].mode()[0], inplace=True)

# Drop columns with a high number of missing values if they are not critical
spotify_data.dropna(subset=['in_shazam_charts'], inplace=True)
```
Data cleaning prepares the dataset by addressing missing values and irrelevant information. Here, the code checks for missing values in each column, helping to determine areas that require cleaning. To handle missing data, certain columns are filled with the most frequent value, while others with substantial missing values are removed if they are deemed uncritical to the analysis. For instance, key values might be filled with the mode (most common value), while rows with missing values in in_shazam_charts could be dropped. This step ensures a complete and consistent dataset, essential for accurate analysis.

_**4. Top Tracks and Artists Analysis**_
```
# Identify the top tracks by streams
# Sorts data to identify which songs have the highest number of streams
top_tracks = spotify_data.sort_values(by='streams', ascending=False).head(10)

# Identify the most frequent artists
# Aggregates data by artist to determine who has the most entries in the dataset
artist_counts = spotify_data['artist'].value_counts()
```
This section focuses on identifying the most popular songs and artists. To determine the top tracks, the dataset is sorted by the streams column in descending order, revealing the highest-streamed songs. For artists, the code counts the occurrences of each unique artist name, providing insight into who appears most frequently in the dataset. This helps in spotting artists who are consistently popular or have multiple hits within the dataset, offering a clear view of leading artists and tracks in terms of streaming numbers.
#### ![output_8_1](https://github.com/user-attachments/assets/81da1d48-9255-4ac1-abab-74b2f6925928)
#### ![output_10_1](https://github.com/user-attachments/assets/61fcef92-7d0c-4ccc-86a5-8cfac04bec2b)

_**5. Release Trends Over Time**_
```
# Convert release dates to datetime format
# This enables time-based analysis, such as monthly or yearly trends
spotify_data['release_date'] = pd.to_datetime(spotify_data['release_date'])

# Plot monthly distribution of releases
# Creates a time series plot to visualize trends in song releases over time
spotify_data['release_date'].dt.to_period("M").value_counts().sort_index().plot(kind='line')
plt.title('Monthly Song Releases')
plt.xlabel('Month')
plt.ylabel('Number of Releases')
plt.show()
```
To analyze trends in song releases over time, this section converts the release_date column into a datetime format, enabling time-based manipulation. Monthly trends are then plotted by grouping the data by month, showing how many songs were released in each period. This plot helps reveal patterns in release activity, such as whether there are seasonal trends in new music releases or spikes in specific months. Such insights are valuable for understanding the music industry’s release strategies and listener engagement trends.
#### ![output_13_0](https://github.com/user-attachments/assets/6763d297-7d8b-4363-b71c-aeef434a135a)
#### ![output_16_1](https://github.com/user-attachments/assets/7b86049f-101e-4137-9dfa-945282da97e0)

_**6. Analyzing Song Characteristics**_
```
# Calculate correlation between song characteristics and streams
# Shows how variables like danceability, energy, and tempo are associated with popularity
correlation = spotify_data[['danceability', 'energy', 'tempo', 'streams']].corr()

# Visualize correlation matrix
# Uses a heatmap to make it easier to see which characteristics impact streams most
sns.heatmap(correlation, annot=True, cmap='coolwarm')
plt.title('Correlation of Song Characteristics with Streams')
plt.show()
```
Here, the focus shifts to understanding how song attributes relate to popularity. The code calculates correlations between song characteristics—such as danceability, energy, and tempo—and the number of streams. A correlation matrix visualizes these relationships with a heatmap, where stronger correlations indicate that certain characteristics may be linked with higher popularity. This analysis helps in identifying traits that are often associated with successful tracks, providing insights into the qualities that listeners might favor in popular music.
#### ![output_19_0](https://github.com/user-attachments/assets/b97567eb-7fee-450b-b48a-99cd440967b1)

_**7. Platform Popularity Comparison**_
```
# Analyze how frequently tracks appear on different streaming platforms
# Counts appearances by platform to gauge where songs are most likely included
platform_counts = spotify_data[['in_spotify_charts', 'in_apple_charts', 'in_deezer_charts']].sum()

# Plot platform popularity
# Visualizes the counts to compare distribution across Spotify, Apple, and Deezer
platform_counts.plot(kind='bar')
plt.title('Platform Popularity of Tracks')
plt.xlabel('Platform')
plt.ylabel('Number of Tracks')
plt.show()
```
This section examines the presence of songs across different streaming platforms. By counting the number of times songs appear on Spotify, Apple, and Deezer, the code reveals the distribution of tracks across these services. A bar plot visualizes these counts, allowing for an easy comparison of each platform’s popularity in terms of song availability. This insight helps in understanding platform-specific trends, which is beneficial for musicians and producers deciding where to promote their music for maximum reach.
#### ![output_22_0](https://github.com/user-attachments/assets/fb7f1dc6-7bda-4cc9-997c-2c215f93f5f5)

_**8. Key and Mode Analysis**_
```
# Analyze distribution of keys in popular tracks
# Checks if certain musical keys are more popular in high-stream tracks
popular_keys = spotify_data[spotify_data['streams'] > spotify_data['streams'].quantile(0.75)]['key'].value_counts()

# Plot key popularity among top-streamed tracks
popular_keys.plot(kind='bar')
plt.title('Key Distribution in Top-Streamed Tracks')
plt.xlabel('Musical Key')
plt.ylabel('Number of Tracks')
plt.show()
```
The analysis of musical keys focuses on understanding whether specific keys are more common among high-stream tracks. By filtering tracks with stream counts in the top 25% and analyzing their key distribution, the code determines if there’s a preference for certain musical keys. This bar plot of key distributions among top-streamed tracks helps reveal patterns or trends in musical composition for popular songs, offering insights into the tonal choices that may appeal to listeners.
#### ![output_25_0](https://github.com/user-attachments/assets/e385e3f0-04cc-4092-b26e-8bbb355723ff)

_**9. Artist Analysis: Playlist Appearances**_
```
# Calculate the number of playlist appearances by artist
# Helps identify which artists are most frequently included across platforms
artist_playlist_counts = spotify_data.groupby('artist')[['in_spotify_charts', 'in_apple_charts', 'in_deezer_charts']].sum()

# Filter to top 10 artists by playlist appearances and plot results
top_artists_playlist = artist_playlist_counts.sum(axis=1).nlargest(10)
top_artists_playlist.plot(kind='bar')
plt.title('Top Artists by Playlist Appearances')
plt.xlabel('Artist')
plt.ylabel('Total Appearances')
plt.show()
```
This section assesses which artists frequently appear on different playlists across platforms. Grouping the data by artist and summing their appearances in the Spotify, Apple, and Deezer charts highlights those with the most playlist placements. The results are filtered to the top ten artists and plotted, providing an overview of who is most present across major streaming platforms. This analysis is particularly useful in understanding the reach and influence of leading artists, as well as in assessing their appeal across multiple listening platforms.
#### ![output_28_1](https://github.com/user-attachments/assets/0ef5ce62-2bef-4ec6-a5e1-930f5b3a0e40)

---
### Data Interpretation

#### Spotify’s Most Streamed Tracks of 2023: Insights and Trends

The Spotify dataset for 2023 highlights streaming habits, artist popularity, genre preferences, and platform-specific playlist dynamics, offering a detailed look into the current landscape of music consumption. The dataset, comprising 953 rows and 24 columns, provides information on top tracks, artists, stream counts, release dates, and musical attributes. Although most columns are complete, there are a few missing entries, particularly in the key and in_shazam_charts columns. This comprehensive dataset allows for a deep analysis of factors that contribute to a track’s popularity and offers a glimpse into how different platforms curate their playlists.

Among the most-streamed tracks, The Weeknd’s “Blinding Lights” leads with an impressive 3.70 billion streams, followed closely by Ed Sheeran’s “Shape of You” with 3.56 billion streams. These numbers illustrate the massive appeal of global pop hits. Other tracks with significant streaming counts include Lewis Capaldi’s “Someone You Loved” and Tones and I’s “Dance Monkey,” demonstrating that listeners continue to favor tracks that blend mainstream pop elements with cross-genre influences [Figure 1.0](https://github.com/user-attachments/assets/81da1d48-9255-4ac1-abab-74b2f6925928). Artists like The Weeknd and Ed Sheeran frequently appear in the dataset, underscoring their consistent popularity and ability to appeal to broad audiences across diverse demographics and listening preferences. [Figure 2.0](https://github.com/user-attachments/assets/61fcef92-7d0c-4ccc-86a5-8cfac04bec2b)

A closer look at release patterns shows that music production has increased steadily from 2021 to 2023, with 2022 seeing the highest volume of new releases [Figure 3.0](https://github.com/user-attachments/assets/6763d297-7d8b-4363-b71c-aeef434a135a). Monthly data for 2023 reveals that January and May were peak release months, suggesting that these periods may align with strategic industry decisions and listener habits. Release counts tend to decline toward the end of the year, possibly as platforms focus more on year-end lists and holiday promotions. Such timing patterns could indicate the influence of seasonal listening trends and strategic release planning in the industry [Figure 4.0](https://github.com/user-attachments/assets/7b86049f-101e-4137-9dfa-945282da97e0).

The dataset also reveals interesting relationships between musical attributes and streaming success. Attributes like bpm, danceability, and energy show negative correlations with streams, especially danceability and speechiness, which tend to correlate with lower stream counts. Acousticness shows the weakest negative correlation, suggesting it has a limited impact on a track’s popularity. These findings hint at a listener preference for tracks with moderate energy levels and more balanced rhythmic qualities, rather than tracks with very high danceability or speechiness scores [Figure 5.0](https://github.com/user-attachments/assets/b97567eb-7fee-450b-b48a-99cd440967b1).

On the platform side, Spotify emerges as the most inclusive, with the highest mean and median track counts in its playlists, suggesting a preference for diverse and expansive playlists. This trend aligns with Spotify’s user base, which seems to favor serendipitous music discovery and a broad range of listening options. Apple Music, however, has lower average track counts per playlist, pointing to a more curated, personalized listening experience that may appeal to users seeking high-quality audio and selective, focused playlists. Deezer, positioned between the two, offers a balanced approach, catering to users who appreciate both larger genre-based playlists and smaller mood-based collections. Spotify’s larger playlists indicate that it tends to favor popular tracks more prominently than its counterparts, which might appeal to listeners eager to explore trending music [Figure 6.0](https://github.com/user-attachments/assets/fb7f1dc6-7bda-4cc9-997c-2c215f93f5f5).

In examining the musical modes and keys of these popular tracks, a pattern emerges: tracks in minor keys, especially E minor and A minor, consistently outperform those in major keys. This suggests a listener preference for the emotional depth often associated with minor keys, perhaps due to their harmonic complexity and alignment with popular genres. Interestingly, certain major keys, like C major and D major, also attract significant streams, though they generally trail behind the minor keys. The popularity of minor keys may reflect broader trends in emotional resonance and cultural preferences among listeners [Figure 7.0](https://github.com/user-attachments/assets/e385e3f0-04cc-4092-b26e-8bbb355723ff).

Finally, an analysis of playlist appearances reveals that The Weeknd is the most frequently featured artist across playlists, reflecting his widespread popularity and the diversity of his music. The list of top playlisted artists spans various genres, including pop, hip-hop, rock, and electronic, underscoring that playlist curation transcends genre constraints and focuses instead on broad listener appeal. This mix of contemporary and classic artists reflects the inclusive nature of playlisting, catering to a wide range of listener preferences and moods. The strong presence of artists like Ed Sheeran and The Weeknd in playlists highlights their ability to engage listeners consistently and remain relevant across platforms [Figure 8.0](https://github.com/user-attachments/assets/0ef5ce62-2bef-4ec6-a5e1-930f5b3a0e40).

Overall, the dataset paints a vivid picture of the music landscape on Spotify in 2023, from the most popular tracks and artists to the ways different platforms promote and curate music. These insights into streaming patterns, musical characteristics, and platform-specific curation strategies provide valuable information for artists, producers, and platforms alike. By understanding these trends, industry professionals can make more informed decisions about release timing, playlist inclusion, and musical direction in a competitive and dynamic streaming environment.

---
### Conclusion
This exploratory data analysis of Spotify’s most-streamed songs of 2023 provided insights into track popularity, artist trends, and platform-specific playlist curation. The Weeknd’s “Blinding Lights” leads with 3.70 billion streams, followed by Ed Sheeran’s “Shape of You” with 3.56 billion, underscoring the global appeal of these artists. Track releases peaked in 2022, with January and May showing the most monthly releases in 2023.

Platform-wise, Spotify features the largest, most varied playlists, indicating a culture of music discovery, while Apple Music favors smaller, more curated playlists. Notably, songs in minor keys like E minor and A minor showed higher streams, suggesting a listener preference for tracks with emotional depth. This EDA highlights valuable patterns for artists and producers to consider, including the importance of aligning musical and release strategies with listener preferences.

---
### License
This project is licensed under The Unlicense. Please see [LICENSE](https://github.com/draemonsi/ECE2112-Exploratory-Data-Analysis-on-Spotify-2023-Dataset/blob/main/LICENSE) file for more details.

---
### Author
Andrei Jorelle C. Simon<br/>
[GitHub Profile](https://github.com/draemonsi)
