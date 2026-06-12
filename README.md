import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns

deliveries = pd.read_csv(r'D:\3rd Year\deliveries.csv.zip')

print("Shape:", deliveries.shape)
print("\nColumns:", deliveries.columns.tolist())
print("\nFirst 3 rows:\n", deliveries.head(3))


# Check what columns we actually have
print(deliveries.columns.tolist())
print(deliveries.shape)
print(deliveries.head(3))

print(deliveries.columns.tolist())

# Total runs per team per match, highest scorer wins
match_runs = deliveries.groupby(['match_id', 'batting_team'])['total_runs'].sum().reset_index()

# Team with highest runs in each match = winner
wins = match_runs.loc[match_runs.groupby('match_id')['total_runs'].idxmax()]
team_wins = wins['batting_team'].value_counts().reset_index()
team_wins.columns = ['team', 'wins']

plt.figure(figsize=(12,6))
sns.barplot(data=team_wins, x='wins', y='team', palette='viridis')
plt.title('IPL All-Time Win Count by Team')
plt.xlabel('Total Wins')
plt.tight_layout()
plt.savefig('team_wins.png', dpi=150)
plt.show()
print(team_wins)

batsmen = deliveries.groupby('batter')['batsman_runs'].sum().reset_index()
batsmen.columns = ['batsman', 'total_runs']
top_batsmen = batsmen.sort_values('total_runs', ascending=False).head(15)

plt.figure(figsize=(12,6))
sns.barplot(data=top_batsmen, x='total_runs', y='batsman', palette='rocket')
plt.title('Top 15 Run Scorers in IPL History')
plt.xlabel('Total Runs')
plt.tight_layout()
plt.savefig('top_batsmen.png', dpi=150)
plt.show()
print(top_batsmen)

# Extract season from match_id (first 4 digits = year)
deliveries['season'] = deliveries['match_id'].astype(str).str[:4]

season_total = deliveries.groupby('season')['total_runs'].sum().reset_index()

plt.figure(figsize=(10,5))
sns.lineplot(data=season_total, x='season', y='total_runs', marker='o', color='orange')
plt.title('Total Runs Scored Per IPL Season')
plt.xlabel('Season')
plt.ylabel('Total Runs')
plt.xticks(rotation=45)
plt.tight_layout()
plt.savefig('season_runs.png', dpi=150)
plt.show()
print(season_total)

team_wins.to_csv('team_wins.csv', index=False)
top_batsmen.to_csv('top_batsmen.csv', index=False)
season_total.to_csv('season_runs.csv', index=False)
deliveries.to_csv('deliveries_clean.csv', index=False)

print("All files exported successfully!")
print("Files saved:", ['team_wins.csv', 'top_batsmen.csv', 'season_runs.csv', 'deliveries_clean.csv'])
