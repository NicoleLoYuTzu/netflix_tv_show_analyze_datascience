# netflix_tv_show_analyze_datascience
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

# 讀取數據
df = pd.read_csv('/kaggle/input/netflix-movies-and-tv-shows/netflix_titles.csv', encoding='ISO-8859-1')
df.head()  # 顯示數據集的前五行

# 刪除不必要的列，只保留前10列
df = df.iloc[:, :10]
df.head()  # 顯示刪除列後的數據集前五行

# 查看數據集的形狀
df.shape  # (8809, 10) 表示數據集中有8809行和10列

# 計算每列的缺失值數量
df.isnull().sum()

# 刪除重複行
df.drop_duplicates(inplace=True)
df.shape  # 查看刪除重複行後的數據集形狀

# 繪製數據集中電影和電視節目的數量
plt.figure(figsize=(10,6))
sns.countplot(x=df['type'], data=df, palette='colorblind')
plt.title('Number of Movies and TV Shows')
plt.show()

# 計算每個國家/地區的節目數量，並提取節目數量最多的前5個國家/地區
country_counts = df['country'].value_counts()
top_5_countries = country_counts.nlargest(5)

# 繪製前5個國家/地區的節目數量條形圖
plt.figure(figsize=(10, 6))
sns.barplot(x=top_5_countries.index, y=top_5_countries.values, palette='colorblind')
plt.title('Top 5 countries by Number of shows (both Movies and TV Shows) produced')
plt.show()

# 篩選出數據集中所有的電影
movies_df = df[df['type'] == 'Movie']

# 計算每個國家/地區生產的電影數量，並提取生產電影數量最多的前5個國家/地區
top_5_movies = movies_df['country'].value_counts().nlargest(5)

# 繪製前5個國家/地區生產的電影數量條形圖
plt.figure(figsize=(10, 6))
sns.barplot(x=top_5_movies.index, y=top_5_movies.values, palette='colorblind')
plt.xlabel('Country')
plt.ylabel('Number of Movies Produced')
plt.title('Top 5 Countries by Number of Movies Produced')
plt.show()

# 篩選出數據集中所有的電視節目
tv_shows_df = df[df['type'] == 'TV Show']

# 計算每個國家/地區生產的電視節目數量，並提取生產電視節目數量最多的前5個國家/地區
top_5_tv_shows = tv_shows_df['country'].value_counts().nlargest(5)

# 繪製前5個國家/地區生產的電視節目數量條形圖
plt.figure(figsize=(10, 6))
sns.barplot(x=top_5_tv_shows.index, y=top_5_tv_shows.values, palette='colorblind')
plt.xlabel('Country')
plt.ylabel('Number of TV Shows Produced')
plt.title('Top 5 Countries by Number of TV Shows Produced')
plt.show()

# 繪製電影發行年份的分佈圖
plt.figure(figsize=(10,6))
plt.hist(df['release_year'], bins=50, color='#0072B2', edgecolor='white')
plt.title('Release year of movies')
plt.show()

# 查看數據集的信息
df.info()

# 將電影時長轉換為數字類型
movies_df['duration'] = movies_df['duration'].astype(str)
movies_df.loc[:, 'duration'] = pd.to_numeric(movies_df['duration'].str.extract(r'(\d+)')[0], errors='coerce')

# 查看轉換後的電影數據集前五行
movies_df.head()

# 繪製電影分級的分佈圖
plt.figure(figsize=(10,6))
sns.countplot(x=df['rating'], data=df, palette='colorblind')
plt.title('Distribution of Rating')
plt.show()

# 繪製電影時長的分佈圖
plt.figure(figsize=(10,6))
plt.hist(movies_df['duration'], bins=50, color='#E69F00', edgecolor='white')
plt.title('Duration distribution of movies')
plt.show()
