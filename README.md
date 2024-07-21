**WhatsApp Caht Sentiment Analysis**
Here's a README for your project "WhatsApp_Chat_Sentiment_Analysis":

---

WhatsApp_Chat_Sentiment_Analysis

This project performs sentiment analysis on WhatsApp chat data, extracting and analyzing the sentiment of messages. The analysis includes calculating positive, negative, and neutral sentiment scores, as well as extracting emojis from the chat messages.

Features

- **Loading and Preprocessing**: Load WhatsApp chat data from a text file and preprocess it to extract dates, times, contacts, and messages.
- **Sentiment Analysis**: Use NLTK's VADER SentimentIntensityAnalyzer to compute positive, negative, and neutral sentiment scores for each message.
- **Emoji Extraction**: Extract and count emojis from messages.
- **Overall Sentiment Calculation**: Calculate the overall sentiment of the chat based on the aggregated sentiment scores.

## Prerequisites

- Python 3.x
- pandas
- nltk
- regex

## Installation

1. Clone the repository:
   ```sh
   git clone https://github.com/yourusername/WhatsApp_Chat_Sentiment_Analysis.git
   cd WhatsApp_Chat_Sentiment_Analysis
   ```

2. Install the required Python packages:
   ```sh
   pip install pandas nltk regex
   ```

3. Download the VADER lexicon for NLTK:
   ```python
   import nltk
   nltk.download('vader_lexicon')
   ```

## Usage

1. Place your WhatsApp chat export text file (e.g., `WhatsApp Chat with Prathap M.txt`) in the project directory.

2. Run the following script to perform sentiment analysis:
   ```python
   import pandas as pd
   import re
   from datetime import datetime
   import nltk
   from nltk.sentiment.vader import SentimentIntensityAnalyzer
   import regex

   # Load the WhatsApp chat data
   file_path = 'WhatsApp Chat with Prathap M.txt'

   # Initialize lists to store the extracted data
   dates = []
   times = []
   contacts = []
   messages = []

   # Read the file and extract the information
   with open(file_path, 'r', encoding='utf-8') as file:
       for line in file:
           # Check for the standard WhatsApp message pattern
           match = re.match(r'(\d{1,2}/\d{1,2}/\d{2,4}), (\d{1,2}:\d{2}\s[APM]{2}) - ([^:]+): (.+)', line)
           if match:
               dates.append(match.group(1))
               times.append(match.group(2))
               contacts.append(match.group(3))
               messages.append(match.group(4))

   # Create a DataFrame
   df = pd.DataFrame({'Date': dates, 'Time': times, 'Contact': contacts, 'Message': messages})

   # Convert 'Date' and 'Time' to datetime objects
   df['Date'] = pd.to_datetime(df['Date'], format='%m/%d/%y')
   df['Time'] = pd.to_datetime(df['Time'], format='%I:%M %p').dt.time

   # Download the VADER lexicon if not already done
   nltk.download('vader_lexicon')

   # Initialize the SentimentIntensityAnalyzer
   sentiments = SentimentIntensityAnalyzer()

   # Perform sentiment analysis on each message
   df['Positive'] = df['Message'].apply(lambda x: sentiments.polarity_scores(x)['pos'])
   df['Negative'] = df['Message'].apply(lambda x: sentiments.polarity_scores(x)['neg'])
   df['Neutral'] = df['Message'].apply(lambda x: sentiments.polarity_scores(x)['neu'])

   # Define the split_count function to extract emojis
   def split_count(text):
       emoji_pattern = regex.compile(r'\p{Emoji}', regex.UNICODE)
       return emoji_pattern.findall(text)

   # Apply the function to extract emojis
   df["emoji"] = df["Message"].apply(split_count)

   # Calculate the total number of emojis in the dataset
   emojis = sum(df['emoji'].str.len())

   # Display the first 50 rows with the emojis extracted
   print(df.head(50))

   # Calculate the total sentiment scores
   x = df["Positive"].sum()
   y = df["Negative"].sum()
   z = df["Neutral"].sum()

   # Define the score function to determine overall sentiment
   def score(a, b, c):
       if (a > b) and (a > c):
           print("Positive")
       elif (b > a) and (b > c):
           print("Negative")
       elif (c > a) and (c > b):
           print("Neutral")

   # Determine the overall sentiment
   score(x, y, z)
   ```

## Contributing

Contributions are welcome! Please open an issue or submit a pull request for any features, bug fixes, or enhancements.



---

Feel free to customize the README as needed for your specific project details.
