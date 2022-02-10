## Personal Dashboard

I've been tracking my mood, activities and location using <a href="https://daylio.net/" target="_blank" rel="noopener noreferrer">Daylio</a> and the Google Maps Timeline for a couple of years and visualized and analyzed this data. For this purpose, I have developed a R Shiny app of which I am sharing a small excerpt with partially anonymized data.

### Activity Correlation

| ![graph network](images/Netzwerk.png) | 
|:--:| 
| *simulation of a physical network of activities where correlation corresponds to bond strength and color corresponds to correlation to mood* |

| ![heatmap of correlation between activities](images/Korrelation Aktivität Folgetag.png) | 
|:--:| 
| *correlation of activities on consecutive days* |

### Mood Distribution

As understanding my own mood is the main focus of this project, I have put a good portion of my effort in visualizing my mood distribution w.r.t. the activities I track.

| ![mood distribution with family without allergy and exam](images/mood distribution with or without activities.png) | 
|:--:| 
| *mood distribution on days with or without selected activites* |

Apparently, sleep quality has a great influence on my mood. If I had a bad night of sleep, my mood is significantly worse than if I hadn't. However, the difference between a good night and an okayish night seems to be almost negligible.

| ![mood distribution by sleep quality](images/mood distribution by activity value.png) | 
|:--:| 
| *mood distribution by sleep quality* |

### Visited Locations

Another part of my personal dashboard consists of visualizing the places that I've been to and the amount/frequency that I travel.

| ![map with custom markers](images/Besuche Hamburg Laeiszhalle.png) | 
|:--:| 
| *some of the places I've visited in Hamburg* |

[GitHub-project](https://github.com/NiklasvonM/Daylio)